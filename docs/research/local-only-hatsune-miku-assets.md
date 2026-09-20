# Hatsune Miku assets for a private local-only Xenrya pack

**Research date:** 2026-09-20  
**Scope:** non-pixel Miku assets that may be used by one developer on a local Xenrya checkout. No asset was downloaded.

## Short answer

A defensible **local-only development pack is feasible**, provided “local-only” is enforced as a real boundary:

- the model is stored outside the repository, for example under `~/.local/share/xenrya/characters/`;
- it is never committed, uploaded, bundled into a release, included in CI, copied into an issue/PR, or used in public screenshots/videos;
- the application loads it only when the developer explicitly points Xenrya at that local path;
- the normal build, tests, smoke harness, and public documentation use an original or CC0 fixture instead;
- the developer retains the source URL, creator credit, and a local copy of the applicable terms.

This does **not** make Miku an MIT-licensed Xenrya asset. Hatsune Miku remains Crypton Future Media’s character, and a fan model also carries the model creator’s separate terms.

## Rights boundary

Crypton’s official [Piapro Character License (PCL)](https://piapro.jp/license/pcl) permits creating and distributing self-created Miku derivative works only subject to its conditions. The text prohibits commercial use or receiving compensation, generally requires a derivative work rather than an unchanged official image, and does not allow the user to sublicense Crypton’s permission. The [PCL guidelines](https://piapro.jp/license/character_guideline) say non-profit, free use is the ordinary permitted case; corporate use and use beyond hobby/school scale require an individual agreement. The guidelines also say official images are not generally licensed as standalone application artwork; the listed exception is a non-profit video using the corresponding Crypton synthesised voice.

For creators outside Japan, Crypton’s [English creator terms](https://piapro.net/intl/en_for_creators.html) describe the original Crypton illustrations as CC BY-NC: copying, adapting, distributing, and transmitting are allowed for noncommercial use with attribution. Those terms apply to Crypton’s original illustrations, not to another fan artist’s adaptation. If a downloaded work is fan-made, both the fan creator’s terms and Crypton’s character terms still matter.

## Candidates

### 1. Official Live2D Cubism Hatsune Miku sample — best 2D lead, private development only

**Source/preview:** [Live2D’s official Hatsune Miku sample page](https://www.live2d.com/en/learn/sample/hatsune-miku/)  
**Rights holder/source:** Live2D sample data based on an original Hatsune Miku illustration; the page identifies Hatsune Miku as an externally licensed character.  
**Format:** `.cmo3` authoring model, `.can3` motions, and an embedding/runtime set containing `.moc3`, `.motion3.json`, `.model3.json`, `.physics3.json`, and `.cdi3.json`. The page describes skinning, physics, and motion.

**Terms found:** See Live2D’s [Free Material License Agreement](https://www.live2d.com/eula/live2d-free-material-license-agreement_en.html) and [sample-data terms](https://www.live2d.com/en/learn/sample/model-terms/).

- Live2D describes the sample collection as being for learning Cubism Editor and testing Cubism SDK integration.
- The page’s broad publication permission expressly excludes Hatsune Miku. The sample terms classify Miku as an externally licensed character and defer to Crypton’s character guidelines.
- Live2D’s material agreement has no-redistribution and no-transfer rules except where expressly permitted. Do not treat the sample as a redistributable Xenrya asset.
- For a private test, retain the Live2D/Crypton notices locally. Do not modify the model unless the applicable sample terms and Crypton’s rules allow the exact change; the safest local test is to use the sample as supplied.

**Attribution/public display:** Keep the Live2D and Crypton notices with the local pack. Do not publish screenshots, recordings, binaries, or a demo that displays it.  
**Xenrya fit:** Visually the closest match to the requested anime companion. Technically high effort: Xenrya currently specifies QML sprite sheets/frame sequences, while this needs a Live2D runtime and a native SDK bridge. Live2D’s [SDK release guidance](https://www.live2d.com/en/sdk/license/) says a release license is not required during trial/development, but release terms must be rechecked if Xenrya ever ships Live2D content.  
**Assessment:** Safest provenance for a **private 2D experiment**, not safe as a Phase 0 bundled character or public test fixture.

### 2. Daily-style Miku — strongest expression-rich VRM candidate

**Source/preview:** [Daily’s creator-published BOOTH page](https://booth.pm/ja/items/7262880)  
**Creator:** Daily (`@zxc99707`).  
**Format:** Free VRM 0.0; the page reports 44 MMD-compatible blendshapes and 21 facial-expression animations. A separate supporter-only VRChat package is also listed; do not use that package unless personally obtained under its terms.

**Terms found:** The creator says the model is a 3D representation of Crypton’s Hatsune Miku based on PCL, and that the free VRM may be modified and uploaded to VRChat for personal use only. The page says a VN3 license is included in the download. Since the archive was not downloaded, the exact VN3 clauses were not independently checked.

**Attribution/public display:** Follow the included VN3 terms and retain the creator/PCL credit. The page does not state a general redistribution grant for the VRM; keep it outside the repo and do not show it in public Xenrya material.  
**Xenrya fit:** Medium-to-high effort. VRM gives useful facial expressions, but Xenrya has no VRM renderer and currently targets 2D frame sequences. A private renderer or offline local preview would be a separate architecture choice.  
**Assessment:** Good practical candidate for a personal local pack after reading the included VN3 file and asking the creator whether “private desktop application” is covered. Do not infer that VRChat permission automatically covers Xenrya.

### 3. MW Miku 3D Model — clearest no-edit/no-redistribution boundary

**Source/preview:** [misfitworks’ creator-published BOOTH page](https://booth.pm/ja/items/6290737)  
**Creators:** 2D reference by Hana (`@mechokkucity`); 3D modelling/setup by Risa (`@misfitworks`).  
**Format:** VRM, VSFAvatar, and Unity package. The page describes a semi-chibi model with an original design; the VSFAvatar is for VNyan/VSeeFace and the VRM has basic physics.

**Terms found:** Non-profit use is allowed; video production and livestreams are listed as allowed. Sexual use, profit-seeking use, NFT/AI/crypto use are prohibited. The model may not be edited and may not be redistributed; sharing should be by linking to the BOOTH page. Credit is required.

**Attribution/public display:** Credit `misfitworks` and follow the page’s restrictions. For Xenrya, keep it private and unmodified; do not use the page’s permission for public Xenrya screenshots or package distribution.  
**Xenrya fit:** Medium-to-high effort because it requires VRM/VSF/Unity-compatible rendering rather than the existing QML frame-sequence path.  
**Assessment:** Strongest simple private-use boundary among the fan models, but it is unsuitable if Xenrya needs custom reaction edits.

### 4. Hatsune Miku V4X by Dahlia — personal-only VRM with alterations allowed

**Source/preview:** [Dahlia’s creator-published VRoid Hub page](https://hub.vroid.com/en/characters/7032412562763742257/models/8129529860751927903)  
**Creator:** Dahlia.  
**Format:** VRM 0.0.

**Terms found:** The creator says alterations are allowed, but commercial use, corporate use, and redistribution are prohibited. VRoid Hub marks avatar use and alterations as allowed, individual commercial use and corporate use as disallowed, redistribution as disallowed, and attribution as required. The creator’s description explicitly says companies/businesses require separate authorization.

**Attribution/public display:** Credit Dahlia; no redistribution of the model or altered model, no corporate use, and no public Xenrya screenshots or demos. The local use must genuinely be the individual developer’s private, noncommercial use.  
**Xenrya fit:** Medium-to-high effort for the same VRM/runtime reason; facial reaction coverage was not specified on the page.  
**Assessment:** A plausible local-only candidate for an individual developer, but less conservative than the official Live2D sample because it is a fan derivative and its terms are creator-specific.

## Official flat art: reference only

Crypton’s [official character page](https://piapro.net/pages/character) and product pages expose Miku artwork and product image downloads. The English creator page says original Crypton illustrations are CC BY-NC for noncommercial copying/adaptation with attribution. However, the PCL guideline’s official-image section limits unchanged official-image use to a narrow non-profit video case involving the corresponding Crypton synthesised voice. Therefore, an official flat image is useful as a visual reference, but it should not be treated as a general-purpose desktop character texture without resolving which Crypton license applies to the intended use.

## Recommended local-only implementation contract

For the requested experiment, define a character source with an explicit trust boundary:

```text
XENRYA_CHARACTER_DIR=~/.local/share/xenrya/local-characters/miku
```

The path should be optional and absent in clean checkouts. The normal Phase 0 path should use a bundled original/CC0 fixture. The local Miku path should be disabled in CI, smoke tests, screenshots, crash reports, and packaged artifacts. Store a local `NOTICE.md` beside the asset containing the exact source URL, creator, retrieval date, license/terms URL, and any required credit.

Do not transform a private Miku model into PNG frames and then commit those frames: rendered or modified outputs can remain derivative assets. If a renderer produces cache files, keep its cache outside the repository too.

## Conclusion

Yes: a private, noncommercial, local-only Miku pack appears defensible as a developer convenience and visual experiment, as long as Xenrya never distributes or publicly presents the Miku asset and the model’s own terms are followed. This is a licensing assessment, not legal advice or a clearance for a future release.

The safest choices are split by purpose:

1. **Safest 2D/provenance choice:** the [official Live2D Cubism Hatsune Miku sample](https://www.live2d.com/en/learn/sample/hatsune-miku/) for private SDK/rendering experiments only. It is the closest to the desired anime presentation, but it requires a new Live2D runtime boundary and must never become a shipped fixture.
2. **Safest practical 3D choice:** [Daily-style Miku](https://booth.pm/ja/items/7262880), after reading its included VN3 file and obtaining creator confirmation that private use in a local desktop application is covered. It has the clearest advertised expression/VRM data and personal-only modification language.
3. **Safest no-edit fallback:** [MW Miku](https://booth.pm/ja/items/6290737), if an unmodified model is enough and the creator’s non-profit/private terms are followed exactly.

None of these candidates is suitable for Xenrya’s MIT repository, public release binaries, CI, or public demo screenshots. If the project later needs a distributable anime character, return to the original-character decision rather than promoting this local-only pack into a product asset.
