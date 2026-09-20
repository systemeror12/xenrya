# Non-pixel anime reference character research

**Date:** 2026-09-20
**Wayfinder ticket:** [Select a freely licensed non-pixel anime reference character](https://github.com/systemeror12/xenrya/issues/5)

## Decision summary

The search found permissively licensed, non-pixel anime assets, but no ready-made asset that is simultaneously a strong desktop-companion design, already animated for idle and reactions, compatible with Xenrya's current Qt/QML frame-sequence plan, and cleanly redistributable in packaged binaries.

Recommendation:

1. **Phase 0:** use [Hana School Girl](https://iamst.itch.io/hana-school-girl) only if a temporary reference is needed. Its transparent PNGs and expression variants fit the current renderer best.
2. **Product-quality route:** commission or draw a Xenrya-owned original layered anime character. This is the recommended direction for a teal-haired virtual-idol-like companion; it avoids franchise/trademark ambiguity and gives Xenrya the exact idle/reaction states it needs.
3. **Later experiment:** evaluate [Garnet Live2D](https://salmon-snake.itch.io/garnet-live2d-a-free-model) or one of the 3D models only after the renderer and runtime licensing are decided. Do not bundle a Live2D runtime or sample material as part of Phase 0 by assumption.

This note does **not** select or download an asset. All pixel-art, franchise, fan-sprite, and “free but no redistribution” options are out of scope.

## License gate

The app is MIT-licensed, but artwork does not become MIT-licensed merely because it is shipped by an MIT application. Keep code and character assets separately licensed and preserve the asset's notice in the binary/source distribution.

- [CC0 1.0 deed](https://creativecommons.org/publicdomain/zero/1.0/) permits copying, modification, and distribution, including commercially, without permission or attribution. It does not clear trademark, publicity, privacy, or provenance issues.
- [CC BY 4.0 deed](https://creativecommons.org/licenses/by/4.0/) permits commercial sharing and adaptation, but requires appropriate credit, a license link, and an indication of changes, with no additional restrictions.
- [Live2D Free Material License](https://www.live2d.com/eula/live2d-free-material-license-agreement_en.html) is not a blanket open-content license. Its terms restrict redistribution, modification, transfer, and sublicensing unless expressly permitted for the particular material. A Live2D model's art license and a Cubism SDK publication license are separate questions.
- [Inochi2D legal information](https://github.com/Inochi2D/inochi2d/wiki/Legal-Info) and the project's [BSD-2-Clause license](https://github.com/Inochi2D/inochi2d/blob/main/LICENSE) make it a possible later runtime for an owned layered character; the artist/rigger still controls the model's art license.

## Ranked ready-made shortlist

| Rank | Candidate and source/preview | License and provenance | Format and motion readiness | Decision risk |
| --- | --- | --- | --- | --- |
| 1 | **Hana School Girl — IAMST** ([creator/download page](https://iamst.itch.io/hana-school-girl), [preview 1](https://img.itch.zone/aW1nLzI2MTA1NzA1LmdpZ%3D%3D/original/SaRQcv.gif), [preview 2](https://img.itch.zone/aW1nLzI2MTA1NzM2LmdpZ%3D%3D/original/ybrhBl.gif)) | Page marks the asset **CC0 1.0** and “No generative AI was used.” | Transparent PNGs; the free `separate.zip` is 3.4 MB. Happy, embarrassed, surprised, annoyed expressions plus separate eye, eyebrow, and mouth variants are already usable as reactions. A paid optional `.clip` source is available, but is not required for the free PNG use. | Best match for the current QML sprite/frame-sequence renderer, but it does not ship a documented idle loop. Add a small code-driven float/blink or render an idle sequence. It is a generic pink-haired schoolgirl, not a teal-haired virtual idol. Keep the page's mock-up background credits out of the pack. |
| 2 | **Garnet Live2D — Salmon Snake Games** ([creator/download page](https://salmon-snake.itch.io/garnet-live2d-a-free-model), [preview/provenance page](https://commons.wikimedia.org/wiki/File:Garnet_Live2d_-_A_Free_Model.png)) | Creator page marks the asset **CC0 1.0** and “No generative AI was used.” | Layered Photoshop file (6.2 MB) and Live2D `.cmo3` model (5.1 MB). The layered art is suitable for authoring idle/reaction frames; the page does not promise a shipped idle/reaction animation set. | Strongest free 2D rigging base, but a `.cmo3` needs a Live2D-compatible runtime. Use the PSD as an input to exported PNG frames if possible; do not assume Cubism model/runtime redistribution is permitted. |
| 3 | **3D Anime Female Mage — Dawn to Dusk Games** ([creator/download page](https://dawn-to-dusk-games.itch.io/3d-anime-female-mage-character), [animation/devlog](https://dawn-to-dusk-games.itch.io/3d-anime-female-mage-character/devlog/1498088/3d-anime-female-mage-version-10)) | Creator page marks the asset **CC0 1.0** and “No generative AI was used.” | Free Tier 1 includes FBX/GLB, textures, 10,222 vertices, 19,596 triangles, and 2048px textures (7.4 MB). The version 1.0 devlog describes 17 animations, including idle, movement, defeat, attacks, and emote controls. | Best ready-made animation coverage, but it needs a 3D renderer/conversion path, not the approved Phase 0 sprite-sheet path. The full Blender/Rigify source is a paid tier; exported skeletons have documented Rigify hierarchy quirks. |
| 4 | **3D Anime Female Adventurer — Dawn to Dusk Games** ([creator/download page](https://dawn-to-dusk-games.itch.io/3d-anime-female-adventurer), [version 1.2 devlog](https://dawn-to-dusk-games.itch.io/3d-anime-female-adventurer/devlog/1310838/3d-anime-female-adventurer-version-12)) | Creator page marks the asset **CC0 1.0** and “No generative AI was used.” | Unity package plus model folder/FBX and textures; 16 blend shapes, two outfits, and 11 listed animations including idle, defeat, getting up, crossed-arms emotes, and blinking. Downloads are 21–28 MB depending on version. | Explicit idle and reaction coverage, but it is a larger RPG-style 3D asset and requires Qt Quick 3D, conversion, or an off-screen renderer. Audit included environmental/weapon assets separately before packaging. |

## Findings and implementation fit

### Hana is the only practical Phase 0 reference

Xenrya currently specifies Qt 6/QML and **sprite sheets/frame sequences** for character animation ([technology stack](../08-technology-stack.md)); the MVP requires an idle animation and at least one reaction ([MVP gating](../03-mvp-and-phase-gating.md)). Hana's separate transparent PNGs and expression parts map directly to that contract. A minimal Phase 0 pack can provide:

```text
character/
├── character.toml
├── sprites/
│   ├── idle/
│   └── reactions/
└── LICENSES/
```

The idle frames would be Xenrya-authored motion (blink, subtle bob, or breathing) applied to CC0 art, while the existing happy/surprised/annoyed states provide visibly distinct event reactions. This is a prototype convenience, not a final character decision.

### Garnet is the strongest 2D art base, not a drop-in runtime asset

The creator explicitly supplies both a layered PSD and a Live2D project and grants CC0 for the character asset. That is unusually useful for a future expressive companion. However, the project file is not evidence that Xenrya may redistribute Cubism's runtime, SDK, sample materials, or encrypted/model tooling. The safer Phase 0 experiment is to render/export frames from the layered art and ship only those verified outputs with a CC0 notice.

### The 3D candidates are technically sound but strategically late

The Mage and Adventurer have the clearest idle/reaction coverage and explicit CC0/no-AI declarations. Their formats are also more useful than a static illustration if Xenrya later adopts a 3D renderer. They are poor Phase 0 references because the current architecture deliberately keeps character animation as data-only frame sequences and aims for an efficient Linux desktop companion. Converting a 3D model to pre-rendered frames would add a content pipeline and lose some of the rig's value.

## Recommended Xenrya-owned original

Commissioning or drawing the final character is the cleanest way to achieve the requested anime quality and a teal-haired virtual-idol direction without copying Hatsune Miku or another franchise character. The brief should request an original design with desktop-companion proportions, a transparent full-body view, and layered source art.

Require the artist agreement to grant Xenrya, at minimum:

- worldwide, perpetual rights to use, modify, animate, render, and redistribute the art in source form and packaged binaries;
- rights to make and distribute character packs, forks, screenshots, promotional material, and derivative poses/expressions;
- delivery of layered PSD, Krita, or equivalent source plus transparent PNG/SVG exports;
- an idle loop and at least three visibly distinct reactions (for example: success, warning, and waiting/error);
- a warranty that the work is original, contains no unlicensed third-party/franchise elements, and has documented provenance;
- an explicit statement about generative tools. Prefer human-created work with no generative-AI dependency or undisclosed training/reference material;
- permission to publish the character under the chosen Xenrya asset license, while keeping code under MIT and recording the asset license separately.

For Phase 0, export the owned layered art to transparent PNG frame sequences. Keep the layered source in a private or source asset archive as appropriate, and record the artist, contract/license, source hash, export tool, and any included fonts/textures in `LICENSES/` or `THIRD_PARTY_NOTICES`. If a later phase needs mesh deformation, evaluate Inochi2D separately; do not couple the Phase 0 renderer to Live2D licensing.

## Resolution for Select a freely licensed non-pixel anime reference character

**Do not select a free ready-made final character yet.** The strongest immediate reference is Hana for the existing renderer; Garnet is the strongest layered 2D alternative once runtime and export boundaries are resolved; the Dawn to Dusk models are deferred 3D options. None is as good a product foundation as a Xenrya-owned original layered anime character. Close the research phase with that recommendation and move the actual character choice into the Phase 0 asset/renderer milestone, where the rights record and idle/reaction acceptance test can be reviewed together.

### Deliberate exclusions

- Hatsune Miku and other franchise/fan assets: no candidate was accepted without an explicit license covering modification, app redistribution, and packaged binaries; “free download” or fan-use language is insufficient.
- Live2D sample/official materials: the official license contains material-specific restrictions and is not equivalent to CC0/MIT.
- Models that allow commercial use but prohibit redistribution, paid-only source files, or require a donation without an explicit asset license.
- All pixel-art candidates, per the current product direction.
