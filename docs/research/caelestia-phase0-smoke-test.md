# Caelestia shell as a Xenrya Phase 0 smoke-test substrate

## Snapshot and question

This note evaluates [`systemeror12/caelestia-shell-xenrya`](https://github.com/systemeror12/caelestia-shell-xenrya) at commit [`0b66f927e799f5e9f272bb6949e2321cddbe6211`](https://github.com/systemeror12/caelestia-shell-xenrya/commit/0b66f927e799f5e9f272bb6949e2321cddbe6211), dated 2026-09-18. The question is whether it can be used as an agent-friendly Phase 0 environment for Xenrya's launch, Qt/QML rendering, Rust/CXX-Qt integration, Wayland/Hyprland behavior, synthetic event reaction, shutdown, and display-coverage checks.

The findings below are evidence about the environment and its boundaries. They do not change Xenrya's product or implementation decisions.

## Executive finding

The repository contains a useful, tested compositor harness, but it is not a drop-in Xenrya test runner.

Its strongest reusable capability is process-level Wayland isolation:

- headless Sway for a fast real-Wayland/layer-surface smoke test;
- nested Hyprland for a visible interactive check with native Hyprland IPC;
- private XDG state and unreachable D-Bus addresses;
- optional `grim` screenshots; and
- PID-based cleanup of the shell and compositor.

The harness currently launches Quickshell's QML shell, not an arbitrary Qt application. The repository itself is C++/QML and has no Rust or Cargo sources, so it cannot directly prove Xenrya's Rust/CXX-Qt boundary. It also has no synthetic event driver or assertion mechanism, and its display setup is one output at scale 1. It should therefore be treated as a reference implementation or substrate for a Xenrya-specific harness adapter, not as a runtime dependency of Xenrya.

The closest Phase 0 use is: run Xenrya's own binary under the same isolated compositor setup, add an explicit readiness/health signal and event fixture, and keep multi-monitor/scaling and hardware/system-mutating checks as separate gates.

## What the repository provides

### A Quickshell/QtQuick shell with a C++ plugin

The fork is a Quickshell configuration: the root [`shell.qml`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/shell.qml) loads QML modules, services, layer-shell windows, and a session lock. Its native extension is built by CMake as a Qt 6 QML plugin; [`plugin/CMakeLists.txt`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/plugin/CMakeLists.txt#L1-L22) requires Qt 6.9 components and C/C++ libraries such as PipeWire, Aubio, and Qalculate. The source tree has QML and C++/header files but no `.rs`, `Cargo.toml`, or `Cargo.lock` files (tree at the pinned commit: [`repository files`](https://github.com/systemeror12/caelestia-shell-xenrya/tree/0b66f927e799f5e9f272bb6949e2321cddbe6211)).

That makes it relevant to Qt/QML and Wayland surface startup, but not to Xenrya's approved Rust + Qt 6/QtQuick/QML + CXX-Qt stack ([Xenrya technology stack](../08-technology-stack.md)). It cannot compile, load, or exercise Xenrya's Rust/CXX-Qt bridge by itself.

### An isolated local smoke script

[`scripts/test-shell.sh`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh) exposes this interface:

```text
scripts/test-shell.sh [--interactive] [--duration SECONDS] [--screenshot FILE] [--skip-build]
```

The script requires `cmake`, `cp`, `find`, and `qs`, then requires `sway` for the default mode or `Hyprland` for interactive mode; screenshots additionally require `grim` ([lines 5-81](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L5-L81)). Without `--skip-build`, it configures an existing checkout with CMake/Ninja if needed and builds it ([lines 83-95](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L83-L95)).

The normal manual build contract is documented in [`README.md`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/README.md#L69-L118): install the listed Quickshell, Qt, Wayland, and system-service dependencies, configure with CMake/Ninja, build, and optionally install. The Nix flake supplies a package/dev shell, but its inputs are a Quickshell Git repository, nixpkgs unstable, and Caelestia-specific packages rather than a Rust/CXX-Qt toolchain ([`flake.nix`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/flake.nix#L329-L443)).

For Xenrya, the useful part is the runtime setup and process lifecycle. The `qs --path` invocation is specific to Quickshell. Quickshell's own command parser documents `--path` for loading a QML config and `--pid` for selecting an instance; it also exposes `ipc call` for `IpcHandler` functions ([official parser source](https://github.com/quickshell-mirror/quickshell/blob/v0.3.1/src/launch/parsecommand.cpp#L779-L821), [instance/IPC options](https://github.com/quickshell-mirror/quickshell/blob/v0.3.1/src/launch/parsecommand.cpp#L918-L939), [IPC commands](https://github.com/quickshell-mirror/quickshell/blob/v0.3.1/src/launch/parsecommand.cpp#L1044-L1117)). A native Xenrya binary would need its own launcher and health/event interface.

### Isolation and cleanup

Before launching, the script creates a short-lived temporary test root with separate runtime, config, data, state, cache, home, blocked-command, compositor-log, and shell-log paths ([lines 97-140](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L97-L140)). It copies the user's Caelestia visual config and selected state into the private tree, rather than writing those locations directly ([lines 142-152](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L142-L152)). It masks a list of host-changing commands and sets both session and system D-Bus addresses to unreachable private paths ([lines 154-181](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L154-L181)).

Cleanup is direct and easy for an agent to reason about: the script stores compositor and shell PIDs, traps exit/signals, kills/waits for the children, and deletes the temporary tree ([lines 101-119](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L101-L119)). The script checks liveness for the requested duration, captures an optional screenshot, then terminates the shell in headless mode ([lines 315-380](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L315-L380)).

The repository's own research note describes the same contract and explicitly says the setup is process/user-state/D-Bus/compositor isolation, not a virtual machine ([`quickshell-isolated-testing.md`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L1-L15), [limitations](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L111-L135)).

## Capability assessment for Xenrya Phase 0

| Concern | Current status | Evidence and boundary |
| --- | --- | --- |
| Qt/QML startup | Partial/yes for Quickshell | The script builds the fork's Qt 6 C++ plugin and starts its QML root with `qs`; the source research note lists import/root loading and layer-surface creation as checks ([source](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L95-L109)). It does not launch a standalone Xenrya Qt application. |
| Rust/CXX-Qt | No direct coverage | The fork has no Rust/Cargo sources and its plugin is C++/Qt ([plugin CMake](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/plugin/CMakeLists.txt#L1-L22)). Xenrya must compile and test this boundary in its own build and executable. |
| Wayland surfaces | Yes in headless mode | The default starts a private headless Sway with `WLR_BACKENDS=headless`, one output, and Pixman rendering, then points Quickshell at the discovered private Wayland socket ([lines 219-277](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L219-L277)). |
| Hyprland IPC/protocols | Yes only in interactive mode | Interactive mode requires a live `WAYLAND_DISPLAY`, starts a nested Hyprland with private IPC sockets, and launches the shell against the nested display ([lines 183-217](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L183-L217), [nested config](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/hyprland-test.lua#L1-L42)). The fork's note says headless Sway does not provide Hyprland IPC or Hyprland-specific protocols ([source](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L81-L88)). |
| Rendering/screenshot | Yes, basic | `--screenshot FILE` runs `grim` against the private Wayland display ([lines 336-341](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L336-L341)). This is an artifact for human/agent inspection; there is no pixel-diff or visual assertion. Headless mode forces `QT_QUICK_BACKEND=software` and `WLR_RENDERER=pixman` ([lines 227-241](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L227-L241), [lines 305-311](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L305-L311)), so it is not a GPU/render-performance check. |
| Synthetic event reaction | No | The harness starts the shell, waits, and checks that it remains alive. It does not create a Git fixture, emit an event, call a Xenrya event bus, or assert a reaction. The Quickshell source has IPC handlers for its own drawers/toasts/Hyprland services ([example](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/modules/Shortcuts.qml#L111-L163)), but that is not a generic Xenrya event injector. |
| Clean shutdown | Yes, process-level | The script kills/waits for both child PIDs and removes the temporary root ([cleanup](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L106-L119)). It does not independently inspect all descendants or prove that application-owned background tasks have stopped. Xenrya's stronger “quit means quit” requirement therefore needs an app-specific assertion ([Xenrya NFR-021](../05-non-functional-requirements.md)). |
| Multi-monitor | No in the current interface | Headless mode hardcodes `WLR_HEADLESS_OUTPUTS=1` ([lines 227-241](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L227-L241)). The nested Hyprland config declares one 1920x1080 monitor ([`hyprland-test.lua`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/hyprland-test.lua#L1-L6)). There is no hotplug or multi-output scenario. |
| Scaling/HiDPI | No in the current interface | The nested monitor uses `scale = 1`; the headless path does not expose a scale/output configuration flag. It cannot establish Xenrya's 125/150/200% or fractional-scaling behavior. |
| Agent automation | Partial | The CLI is deterministic and noninteractive by default, returns a process result, and can emit a screenshot. It has no JSON/JUnit report, readiness protocol, event-driver hook, or scenario manifest. The current scheduled GitHub workflow uses a separate simple Sway smoke path with sleeps, `pgrep`, and `killall`, rather than invoking `scripts/test-shell.sh` ([workflow](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/.github/workflows/update-flake-inputs.yml#L19-L38)). |

## Exact setup and run paths

### Local source path

The repository documents a dependency-heavy Arch-style setup: Quickshell Git, Qt 6 modules, CMake/Ninja, Wayland/system services, fonts, and several Caelestia plugins/services ([dependency list](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/README.md#L69-L101)). The documented run command is `caelestia shell -d` or `qs -c caelestia -n -d` ([usage](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/README.md#L136-L158)).

The isolated harness is more useful for development than installing the shell system-wide:

```sh
scripts/test-shell.sh
scripts/test-shell.sh --duration 15
scripts/test-shell.sh --screenshot /tmp/caelestia-test.png
scripts/test-shell.sh --interactive
scripts/test-shell.sh --interactive --skip-build
```

The first four forms are documented in the fork's research note ([commands and prerequisites](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L1-L46)). `--interactive` is for a visible nested Hyprland preview and requires a live Wayland socket; it is not a headless CI path ([script checks](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L183-L199)).

### CI/build paths already present

The repository's build workflow builds a Nix package and a CMake project in an Arch environment using GCC and Clang's `clazy` ([`.github/workflows/build.yml`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/.github/workflows/build.yml#L17-L82)). Its lint workflow builds, uses `QT_QPA_PLATFORM=offscreen` to generate QML tooling, and runs QML/C++ linting ([`.github/workflows/lint.yml`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/.github/workflows/lint.yml#L19-L75)). The repository's research note explicitly warns that offscreen mode is useful for tooling but is not a valid full-shell runtime acceptance test because layer-shell backends are unavailable ([offscreen limitation](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L68-L83)).

The scheduled workflow's headless Sway smoke test installs Sway, starts it, starts the packaged shell, checks for a Quickshell process, then kills the shell and Sway ([`update-flake-inputs.yml`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/.github/workflows/update-flake-inputs.yml#L19-L38)). This demonstrates that the project already uses a real-Wayland smoke check in automation, but its sleep/`pgrep` approach is weaker than a Xenrya-specific readiness and event assertion.

## What is missing for Xenrya

The current script is deliberately tailored to the Caelestia shell:

1. It hardcodes `build/`, checks for `build/qml`, sets `QML2_IMPORT_PATH` and `CAELESTIA_LIB_DIR`, and launches `qs --path "$repo_root"` ([build and environment](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L83-L95), [launch](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L279-L313)). A Xenrya adapter must replace that with a built binary or Cargo command and its runtime library/assets.
2. It has no stable readiness signal. “Still alive after N seconds” catches an early crash but cannot prove that the Qt window, CXX-Qt object registration, event bus, or character renderer reached a usable state.
3. It has no event fixture or driver. Phase 0's synthetic event/reaction path must come from Xenrya, such as a test-only command/socket or a deterministic fixture process.
4. It has no machine-readable result beyond exit status and text logs. Agents can consume exit status and screenshots, but structured step results would make failures easier to triage.
5. It has no display matrix. Xenrya's required single/multi-monitor, hotplug, and 100/125/150/200% checks need a separate configurable compositor scenario.
6. It has no application-level shutdown assertion. PID cleanup can mask a child that ignores graceful shutdown or leaves a watcher/task alive after the top-level process exits.
7. It is not a VM or full system sandbox. The repository notes that kernel interfaces and networking remain available, system D-Bus is blocked, PipeWire is hidden, and network clients are not sandboxed ([limitations](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L111-L135)).

## Coupling risks

### Runtime and dependency coupling

Using the repository as a required Xenrya runtime would couple Xenrya to Quickshell, Caelestia's QML/plugin graph, and its service assumptions. The fork's flake follows a Quickshell Git input and nixpkgs unstable ([`flake.nix`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/flake.nix#L329-L443)); Quickshell's official distribution guide warns that it is still early-stage and may have API breaks ([official guide](https://quickshell.org/docs/v0.3.1/guide/distribution/#api-breaks)). Xenrya should pin any borrowed harness commit and dependency image, and should not make its product architecture depend on Quickshell.

### Test-validity coupling

The default test is software-rendered, one-output Sway. Interactive Hyprland is visible and useful but needs a live host Wayland connection, and the nested config still specifies one output at scale 1. Passing this test would be evidence of startup/layer-surface compatibility, not evidence of Xenrya's Rust/CXX-Qt event loop, GPU rendering, multi-monitor geometry, fractional scaling, or full Hyprland integration.

### State and reproducibility coupling

The script copies the operator's Caelestia config and selected state into each test run. That is convenient for a visual preview but can make an agent result depend on a human's wallpaper/config and can read files outside the Xenrya checkout. A Xenrya test should use a minimal fixture config and controlled assets, with any host-state copy opt-in.

### Isolation and safety coupling

The command blocklist is defense in depth, not a security boundary. Network access, kernel interfaces, and some external services remain reachable. System-changing, hardware-facing, or credential-sensitive cases should not be run through this harness. Use a disposable login session or VM for those cases, consistent with the fork's own recommendation ([development order](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/docs/research/quickshell-isolated-testing.md#L137-L145)).

### Licensing and ownership coupling

The fork declares GPL-3-only licensing in its Nix metadata ([`nix/default.nix`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/nix/default.nix#L150-L155)) and includes the GPL text ([`LICENSE`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/LICENSE)). Copying substantial script/source into Xenrya's MIT repository should be reviewed for licensing and maintenance implications. Keeping the repository as an external, pinned test dependency or reimplementing the small compositor-launch contract avoids silently importing the whole shell.

### CMake shallow-checkout trap

The harness's automatic configure command does not pass `VERSION` or `GIT_REVISION` ([script](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/scripts/test-shell.sh#L83-L89)), while the root CMake file falls back to `git describe --tags` and `git rev-parse HEAD` when those variables are undefined ([`CMakeLists.txt`](https://github.com/systemeror12/caelestia-shell-xenrya/blob/0b66f927e799f5e9f272bb6949e2321cddbe6211/CMakeLists.txt#L1-L20)). A shallow clone without tags therefore needs a preconfigured build or explicit version/revision flags. A Xenrya CI adapter should make source revision inputs explicit rather than relying on repository metadata.

## Recommended role in Phase 0 verification

Use this repository as a pinned reference for an isolated Wayland smoke harness, with a Xenrya-specific launcher layered on top. The reusable contract should be the smallest subset that:

- starts headless Sway for a real Wayland/layer-surface check;
- optionally starts nested Hyprland for a visible Hyprland-specific check;
- provisions private XDG runtime/config/data/state/cache directories and blocked/unreachable service endpoints;
- launches the Xenrya binary with its Rust/CXX-Qt/QML dependencies;
- waits for an explicit Xenrya readiness signal;
- injects one deterministic synthetic event and observes one deterministic reaction;
- captures an optional screenshot;
- sends a graceful shutdown request, waits, escalates only if needed, and asserts no owned process remains; and
- reports structured step results plus logs/artifacts.

Keep these checks separate from the current fork's shell-specific build and state. Use additional compositor scenarios for multi-monitor and scaling. Keep Rust/domain behavior in Xenrya tests that do not launch Qt, matching Xenrya's architecture and testing guidance ([architecture](../07-architecture.md), [testing](../12-testing-quality.md)).

This gives the Caelestia environment a concrete Phase 0 role: a useful Wayland/Hyprland host and visual smoke-test model, not the proof of the entire Phase 0 gate.

## Validation performed against the pinned snapshot

The following was run locally against commit `0b66f927e799f5e9f272bb6949e2321cddbe6211`:

- A fresh depth-1 clone's first `scripts/test-shell.sh --duration 1` failed during CMake configure because the checkout had no tag for `git describe`. This reproduces the shallow-checkout trap above.
- After configuring with explicit `-DVERSION=1.0.0` and `-DGIT_REVISION=0b66f927e799f5e9f272bb6949e2321cddbe6211`, `cmake --build build` completed successfully.
- `scripts/test-shell.sh --skip-build --duration 1` passed with `Isolated Quickshell smoke test passed after 1 seconds.`
- `scripts/test-shell.sh --skip-build --duration 1 --screenshot <temporary PNG>` passed and produced a valid 1280x720 PNG. This confirms the current headless path can render an artifact, not that the artifact matches Xenrya's visual contract.
- `scripts/test-shell.sh --skip-build --interactive --duration 1` launched nested Hyprland successfully. Sending Ctrl+C produced the documented interactive stop path, and no matching nested Hyprland/Quickshell processes remained afterward.

These runs validate the fork's existing shell smoke path only; they do not constitute Xenrya Phase 0 acceptance.
