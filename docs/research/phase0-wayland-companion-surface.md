# Phase 0 Wayland companion-surface strategy

## Question and decision

This note resolves issue [#6](https://github.com/systemeror12/xenrya/issues/6): which maintained Qt 6 and QML-compatible Wayland surface approach should Xenrya use for its Phase 0 character window on Hyprland? The source snapshot is 2026-09-20. The implementation constraints come from Xenrya's [technology stack](../08-technology-stack.md), [architecture](../07-architecture.md), and [Phase 0 gate](../03-mvp-and-phase-gating.md): Rust owns domain and policy, QtQuick/QML owns presentation, Linux/Wayland is the first platform, and the platform-specific surface must sit behind a `CompanionSurface` boundary.

### Recommendation

Use KDE's maintained [`LayerShellQt`](https://github.com/KDE/layer-shell-qt) integration over the `wlr-layer-shell` protocol for the Linux/Hyprland implementation. Use its QML attached properties on a normal QtQuick `Window`, with the following Phase 0 policy:

- put the character on the `top` layer;
- set keyboard interactivity to `none` and do not activate the surface when it is shown;
- use no exclusive zone, because the character is an overlay rather than a panel;
- select an explicit `QScreen`/Wayland output before showing the surface;
- use top and left anchors with margins as the screen-local position; and
- keep the semantic position, visibility, size, selected display, and interaction state in the platform-neutral Rust/CXX-Qt surface model.

The Linux adapter can map that model to `LayerShellQt::Window` in QML. Rust should not receive `QWaylandWindow`, `LayerShellQt`, or any other Qt private/platform handle. An ordinary xdg-shell `Window` remains a useful fallback for an unsupported compositor and for ordinary settings or diagnostics, but it should not be the primary Phase 0 character surface. The Phase 0 Hyprland acceptance path should require the layer-shell adapter so that a passing test proves the intended companion semantics.

This choice accepts one contained risk: LayerShellQt uses Qt Wayland client-private APIs internally. KDE maintains the adapter, Arch packages it, and the adapter isolates that risk to one QML/Qt integration seam. Pin and test a compatible Qt/LayerShellQt pair rather than assuming that every Qt 6 minor release is interchangeable.

## Why layer-shell matches the Phase 0 behavior

The upstream [`wlr-layer-shell-unstable-v1.xml`](https://raw.githubusercontent.com/swaywm/wlroots/0855cdacb2eeeff35849e2e9c4db0aa996d78d10/protocol/wlr-layer-shell-unstable-v1.xml) defines a surface placed in one of the compositor's background, bottom, top, or overlay layers. It also defines an optional output, anchors, size, margins, exclusive zone, and keyboard-interactivity state. These are the controls a desktop companion needs. The protocol does not promise arbitrary global desktop coordinates, which is why the adapter should model position relative to a selected output rather than expose a fake global `x` and `y` API.

Hyprland's current source generates and implements `wlr-layer-shell-unstable-v1` ([protocol generation](https://github.com/hyprwm/Hyprland/blob/83cf6a6ed540dc37808434259c6a3ba663de9616/CMakeLists.txt), [layer-shell protocol](https://raw.githubusercontent.com/hyprwm/Hyprland/83cf6a6ed540dc37808434259c6a3ba663de9616/src/protocols/LayerShell.cpp), and [layer-surface mapping](https://raw.githubusercontent.com/hyprwm/Hyprland/83cf6a6ed540dc37808434259c6a3ba663de9616/src/protocols/LayerSurface.cpp)). Its layer-surface tests cover focus preservation, visibility, and layer rules ([current `hyprtester` layer tests](https://raw.githubusercontent.com/hyprwm/Hyprland/83cf6a6ed540dc37808434259c6a3ba663de9616/hyprtester/src/tests/main/layer.cpp)). This makes layer-shell a native Hyprland path, not a window-rule workaround.

### Focus and input

The protocol's `keyboard_interactivity = none` state tells the compositor never to assign keyboard focus to the surface. The same protocol still delivers pointer, touch, and tablet events to a surface with keyboard interactivity disabled ([protocol input and keyboard sections](https://raw.githubusercontent.com/swaywm/wlroots/0855cdacb2eeeff35849e2e9c4db0aa996d78d10/protocol/wlr-layer-shell-unstable-v1.xml)). That gives the companion pointer dragging and clicking without making the character steal typing focus from the editor or terminal.

LayerShellQt's current API exposes `keyboardInteractivity`, `activateOnShow`, `anchors`, `margins`, `exclusionZone`, `layer`, and `screen` as attached properties. Its current default keyboard mode is on-demand, so Xenrya should set `KeyboardInteractivityNone` explicitly rather than relying on a default ([pinned `Window` API](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/src/interfaces/window.h)). A future preferences or command palette surface can use a separate focused xdg window, or an intentionally interactive layer surface, without changing the character's policy.

### Positioning and dragging

Use a top-left anchored layer surface with an explicit size. Store the companion's position as logical, output-local coordinates in Rust. The QML adapter converts that position to layer-shell margins and clamps it to the selected screen's usable geometry. Pointer handlers update the semantic position during a drag, then update the attached margins. The protocol makes layer state double-buffered and allows margins to be changed with later commits, so this is a suitable implementation model. It is an adapter-level design choice, not a claim that Wayland provides arbitrary global window coordinates.

LayerShellQt's QML test demonstrates the intended shape: a regular QtQuick `Window` imports `org.kde.layershell 1.0` and sets attached anchors, layer, exclusion zone, and margins ([pinned QML test](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/tests/quicktest.qml)). The integration is per-window. Its implementation obtains the Qt Wayland window and attaches a layer-shell integration when the platform surface is created ([pinned implementation](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/src/interfaces/window.cpp)). That is another reason to keep the mechanism in the QML/Qt adapter and out of Rust domain code.

An ordinary top-level Qt window cannot provide the same guarantee. Qt documents that top-level positions are not supported on every windowing system and that, on Wayland, `QWindow::startSystemMove()` is the platform-supported way to ask the compositor for a user-initiated move ([`QWindow` positioning and move docs](https://doc.qt.io/qt-6/qwindow.html)). QML's `Window.x` and `Window.y` can therefore be unavailable or compositor-controlled ([QML `Window` docs](https://doc.qt.io/qt-6.8/qml-qtquick-window.html)). `Qt::WindowStaysOnTopHint` is only a hint ([Qt window flags](https://doc.qt.io/qt-6/qt.html)), so it cannot replace the top layer. An xdg fallback can support a user drag through `startSystemMove()`, but it cannot promise the initial placement, stacking, or autonomous movement required of the primary companion surface.

### Multi-monitor and scaling

The selected display belongs in the semantic surface state as a display identity or user-facing display selection, not as a global coordinate. Layer-shell accepts an output at surface creation, and LayerShellQt exposes a `QScreen *screen` property. Set it before showing the window and update the adapter when the display changes. Hyprland maps the requested output to a monitor and sends output scale and transform information to the surface ([current Hyprland layer-surface source](https://raw.githubusercontent.com/hyprwm/Hyprland/83cf6a6ed540dc37808434259c6a3ba663de9616/src/protocols/LayerSurface.cpp)).

Qt's high-DPI model uses device-independent coordinates and exposes the per-screen/device ratio through `QScreen` and `QWindow`; its Wayland documentation also describes fractional scaling as a case where the ratio may be non-integer ([Qt high-DPI overview](https://doc.qt.io/qt-6.10/highdpi.html), [Qt 6.8 Wayland high-DPI notes](https://doc.qt.io/qt-6.8/highdpi.html)). The surface adapter should therefore:

- keep Rust positions and sizes in logical units;
- use the current `QScreen` and `devicePixelRatio` for raster assets and hit testing;
- react to `screenChanged` and scale changes by recomputing the layer-shell size/margins; and
- avoid assuming that two monitor coordinate spaces form one contiguous global rectangle.

The Phase 0 smoke matrix should cover one output at scale 1, a second output, and at least one fractional or 2x scale. The existing [Caelestia shell smoke-test note](caelestia-phase0-smoke-test.md) records that the supplied harness currently covers one output at scale 1, so it is a useful launch and screenshot substrate but does not itself prove this display matrix.

## Qt, QML, Rust, and CXX-Qt boundary

LayerShellQt is a Qt client integration, not a Rust binding. Its build uses Qt Wayland client integration and the wlr-layer-shell protocol, and installs a `layer-shell` Wayland shell-integration plugin plus the `org.kde.layershell` QML module ([pinned LayerShellQt build](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/CMakeLists.txt), [source target and plugin](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/src/CMakeLists.txt), [QML attached type](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/src/declarative/types.h)).

CXX-Qt's upstream documentation describes Rust QObjects that are usable from C++, QML, and JavaScript, with generated properties, signals, and slots ([CXX-Qt repository at the current source snapshot](https://github.com/KDAB/cxx-qt/tree/1009f849f696e61e26e3ff706b6c4435fd9ddc82), [QML module build example](https://kdab.github.io/cxx-qt/book/getting-started/4-cargo-executable.html)). Use that capability for the semantic `CompanionSurface` object. A practical Phase 0 split is:

```text
Rust/CXX-Qt CompanionSurface
  visible, logical position, size, selected display, interaction policy
  signals/invokables for drag, click, show, hide, and close
             |
             v
QtQuick/QML CompanionSurface adapter
  Linux/Wayland: Window + org.kde.layershell attached properties
  fallback:     ordinary QtQuick Window and compositor-managed move
             |
             v
Qt Wayland QPA / compositor
```

The Rust object should know that it has a display, position, size, and focus policy. It should not know whether those are implemented with layer-shell, xdg-shell, Win32, or another future platform surface. This keeps the boundary usable when Xenrya adds another desktop platform and lets the Linux adapter absorb Qt private API or compositor-version changes.

## Option comparison

| Option | What it gives Xenrya | Main Phase 0 gap or risk | Decision |
| --- | --- | --- | --- |
| KDE LayerShellQt over wlr-layer-shell | Maintained Qt 6 integration, QML attached properties, explicit output, anchors, margins, layer ordering, keyboard policy, and an Arch package. | Uses Qt Wayland client-private APIs; Qt and package versions must be pinned and tested. | **Use for the Hyprland character surface.** |
| Ordinary QtQuick `Window` using xdg-shell | Ships with Qt, works on more Wayland compositors, and is simple to package and expose through CXX-Qt/QML. | Wayland controls placement; topmost is only a hint; user drag can use `startSystemMove()`, but deterministic placement and autonomous movement are unavailable. | Keep as a degraded fallback and for focused non-companion windows. |
| Direct generated wlr-layer-shell client | Qt can generate Wayland protocol client sources through its Wayland client tooling ([Qt Wayland client docs](https://doc.qt.io/qt-6/qtwaylandclient-index.html)). It avoids depending on LayerShellQt's wrapper. | Reimplements surface lifecycle, Qt window integration, QML exposure, configure/scale handling, input, and protocol-version details. The unstable `wlr-` protocol then becomes Xenrya's maintenance burden. | Defer. Revisit only if LayerShellQt blocks a required behavior. |
| `tfbogdan/qtlayershell` | A Qt layer-shell API with QML-like `LayerView`. | The upstream repository's latest commit is from 2018, its README warns about private QtWayland APIs, and it requires process-level shell-integration environment setup before `QGuiApplication` ([pinned repository](https://github.com/tfbogdan/qtlayershell/tree/969683886273d1b2f4731aad4e2de6084c679ca8), [README](https://raw.githubusercontent.com/tfbogdan/qtlayershell/969683886273d1b2f4731aad4e2de6084c679ca8/README.md)). | Reject for Phase 0 maintenance and version risk. |
| `lirios/qtshellintegration` | Qt 6.6-era layer-surface and session-lock interfaces with QML attached properties ([README](https://raw.githubusercontent.com/lirios/qtshellintegration/e4a882542534a8a9fa862889def4df8c0235060f/README.md)). | Smaller project, last source update in 2024, version `0.0.0`, private Qt APIs, and an additional Liri CMake dependency. Session lock is outside Phase 0. | Keep as a research fallback, not the default. |
| Qt Wayland Compositor / QtShell | A Qt API for implementing a compositor or a trusted Qt compositor-to-client extension ([QtShell docs](https://doc.qt.io/qt-6/qtwayland-compositor-qtshell-qmlmodule.html)). | Xenrya is a client of Hyprland, not a compositor. Adopting this would reverse the architecture and add an out-of-scope compositor. | Reject. |

`gtk-layer-shell` is also not a useful alternative here because Xenrya's approved presentation stack is QtQuick/QML. A GTK layer-shell binding would create a second UI toolkit rather than solve the Qt integration problem.

## Maintenance and packaging plan

LayerShellQt is the best-supported option found for this stack, but it is not a zero-risk dependency. At the pinned current upstream commit, its CMake file requires Qt 6.11 and KDE Frameworks 6.30, and its source links `Qt::WaylandClientPrivate` ([pinned CMake](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/CMakeLists.txt), [pinned interface target](https://raw.githubusercontent.com/KDE/layer-shell-qt/440e99dfff02460561a61312f4d90d5cc470b21d/src/CMakeLists.txt)). The Arch Extra package page currently lists a 6.7.5 release and an optional Qt Declarative dependency for QML ([Arch package metadata](https://archlinux.org/packages/extra/x86_64/layer-shell-qt/)). That difference is a concrete warning against pairing an arbitrary distro package with an arbitrary Qt SDK.

For Phase 0:

1. Choose and record one tested Qt 6 plus LayerShellQt release pair. If using Arch Extra, use the LayerShellQt release built for that Qt stack. If building from newer KDE sources, record the required Qt/KF versions and build them in the same reproducible environment.
2. Treat the `layer-shell` Qt platform plugin and `org.kde.layershell` QML module as runtime dependencies. Test both a development run and the packaged run, since a missing plugin can look like an ordinary QML or Wayland startup failure.
3. Keep the Linux adapter small and covered by a real Hyprland smoke test. The existing Caelestia harness can provide an isolated compositor process and screenshots, but the Xenrya binary must be the process under test and must add its own readiness, input, event, and shutdown assertions.
4. Do not vendor a private Qt Wayland implementation in the Rust crate. If the KDE adapter breaks against a future Qt release, the replacement belongs behind the same QML surface adapter.

CXX-Qt itself supports Qt 6 and multiple host platforms, but its project documentation warns that it is still in early development and can change APIs ([upstream README](https://github.com/KDAB/cxx-qt/tree/1009f849f696e61e26e3ff706b6c4435fd9ddc82)). Pin the CXX-Qt version as part of the same build matrix, while keeping it independent of the layer-shell choice.

## Phase 0 acceptance shape

The implementation should be considered ready when the following behavior is observable through the public `CompanionSurface` contract:

- the binary launches on Hyprland with the LayerShellQt QML module available;
- the character appears on the selected output, above ordinary windows, without taking keyboard focus;
- pointer input reaches the character and a drag updates its output-local logical position;
- hide/show preserves the semantic position and does not unexpectedly focus the surface;
- switching the selected output or changing scale recomputes the surface without a pixel-density jump;
- a normal focused window still receives keyboard input while the character is visible; and
- the adapter can be replaced with an ordinary-window implementation in a test or unsupported-compositor environment without changing Rust domain code.

The first implementation should not promise arbitrary desktop-global coordinates or compositor-independent always-on-top behavior. Those are not guaranteed by Wayland's xdg-shell model, and layer-shell expresses position through output, anchors, margins, and configure cycles. The proposed boundary makes that constraint explicit while retaining enough behavior for the Phase 0 character prototype.

## Resolution

Adopt **KDE LayerShellQt over wlr-layer-shell for the Linux/Hyprland Phase 0 companion surface**, isolated behind a platform-neutral Rust/CXX-Qt `CompanionSurface` model and a QML adapter. Configure a non-exclusive top-layer surface with keyboard interactivity disabled, explicit output selection, and anchor/margin positioning. Keep an ordinary QtQuick xdg window as a degraded fallback and for focused utility windows. Reject direct protocol code, stale qtlayershell, lower-activity Liri integration, and Qt compositor APIs for Phase 0. Pin the tested Qt/LayerShellQt/CXX-Qt versions and verify the packaged plugin and QML module in the Caelestia-backed smoke environment.
