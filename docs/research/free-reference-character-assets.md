# Free reference character assets for Phase 0

## Question

Find a freely redistributable chibi or pixel-art character for Xenrya's Phase 0 bundle. It must have an idle animation and at least one visibly distinct reaction, be usable from a packaged MIT-licensed application, have clear attribution/modification terms, and have no known AI-generated provenance where the source page makes that information available.

This is a research shortlist, not an asset selection. No asset archive was downloaded or inspected during this pass. The source pages and preview URLs below are provided for human review.

Snapshot: 2026-09-20.

## Licensing gate

The safest Phase 0 choices are creator-labelled **CC0 1.0 Universal** assets. The [official CC0 deed](https://creativecommons.org/publicdomain/zero/1.0/) permits copying, modification, distribution, and commercial use without asking permission. It does not waive trademark, patent, publicity, or privacy rights, and Creative Commons does not verify the copyright status of works to which a creator applies CC0; see the [CC0 legal code](https://creativecommons.org/publicdomain/zero/1.0/legalcode.en).

For whichever asset is eventually chosen:

- Verify that the downloaded archive contains the expected asset and any license/readme file before committing it.
- Pin the source page, download URL, release date/version, and a SHA-256 hash in a Xenrya third-party-notices record.
- Keep an attribution/source note in the binary distribution even when CC0 makes attribution optional. This reduces provenance ambiguity and does not imply creator endorsement.
- Treat an itch.io page's “No generative AI was used” label as a creator disclosure, not an independent chain-of-title audit. An absent disclosure is a reason to rank an asset lower, not proof that it used AI.

The Xenrya repository is MIT-licensed; the character remains under its own asset license and should not be relicensed as MIT merely because it is bundled with Xenrya.

## Ranked shortlist

### 1. [Pixel Penguin 32x32 Asset Pack](https://legends-games.itch.io/pixel-penguin-32x32-asset-pack) — Legends-Games

**Why it fits:** A small, recognisable, companion-like penguin with an explicit `Idle` and visibly distinct `Hurt` animation. The creator describes it as 32×32 pixel art and says it is free to use on any project. The page's metadata labels it CC0 and says no generative AI was used.

**Technical details:** 32×32 character cells; the page offers a PNG spritesheet plus PSD and XCF source files. The page does not state frame counts or timing. [Preview/spritesheet PNG](https://img.itch.zone/aW1nLzQ3NjU2NTIucG5n/original/nhZWX1.png).

**License and attribution:** Creator page labels the asset [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/). CC0 imposes no attribution requirement; a Xenrya source note is still recommended.

**Risks:** The page calls it a “simple character” and does not include a standalone license text or animation timing. Confirm the archive contents and choose a deterministic frame rate before bundling.

### 2. [16x16 Character](https://alexs-assets.itch.io/16x16-character) — Alex's Assets

**Why it fits:** The most compact integration candidate. It explicitly includes `idle`, `run`, `attack`, `jump`, `fall`, and `death`, so an event can visibly switch from idle to attack or death without inventing frames.

**Technical details:** 28 sprites total; each frame is 16×16; supplied as one spritesheet and individual transparent PNG files. [Animated preview GIF](https://img.itch.zone/aW1nLzIzODQzNjUuZ2lm/original/BlkOG%2B.gif).

**License and attribution:** Creator page labels the asset [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) and marks it “No generative AI was used.” No attribution is required by CC0; retain a source note anyway.

**Risks:** 16×16 is intentionally tiny for a desktop overlay, so the character may need nearest-neighbour scaling and careful anchoring. The page does not state frame timing or show a separate license file; verify both in the archive.

### 3. [Pixel Art Platformer Character](https://pixelgearz.itch.io/pixel-art-platformer-character) — pixelgearz

**Why it fits:** The creator lists a four-frame idle, seven-frame hit reaction, and death animation, plus run/jump/fall/wall-slide/climb/dash. That gives Phase 0 a clear idle-to-hit reaction without needing to edit the art.

**Technical details:** Described as an animated 16px character; the page does not clarify whether that means a 16×16 cell or character height. Download is a 5.2 kB ZIP named `Platformer Character.zip`. [Animated preview GIF](https://img.itch.zone/aW1nLzExODcyMzMwLmdpZg==/original/FcVk3e.gif).

**License and attribution:** Creator page explicitly applies [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/), permits commercial use, says attribution is not mandatory, and marks the work “No generative AI was used.”

**Risks:** The page does not state the inner image format, frame dimensions, or timing. Verify those facts and whether the hit/death poses read well at the intended overlay scale.

### 4. [Customizable Character Pack](https://ordinary-bumblebee.itch.io/customizable-character-pack) — Ordinary Bumblebee

**Why it fits:** A cute 32×32 pixel character with `Idle`, `Hurt`, `Interact`, attacks, walk, run, and jump in four directions. The hurt or interact animation can serve as the first visible event reaction, while the customization options allow Xenrya to avoid looking like a generic platformer protagonist.

**Technical details:** 32×32 cells; eight animations, each in four directions; more than 2.5 million combinations of skin, hair, eyes, and clothing are advertised. The free download is a 699 kB ZIP; inner file formats and timing are not stated. [Animated preview GIF](https://img.itch.zone/aW1hZ2UvMzQ1OTYwNC8yMDYzNzk1MC5naWY=/original/fEYqsh.gif).

**License and attribution:** Creator page labels it [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) and marks it “No generative AI was used.” No attribution is required by CC0; preserve a source note.

**Risks:** Four directions and modular layers add unnecessary Phase 0 integration surface. The archive format and exact frame layout must be verified before adopting it; do not bundle every variation merely because the pack contains them.

### 5. [Pixel Dino Spritesheet — Idle, Run, and Death Animations](https://voidcordtech.itch.io/dino-spritesheet-animation) — Voidcord

**Why it fits:** A cute, high-contrast dinosaur is a strong desktop-companion silhouette. The page supplies `Idle`, `Run`, and `Die`, making the death animation a visually distinct reaction if Phase 0 wants a stronger test signal.

**Technical details:** 128×128 cells; PNG and GIF exports plus an Aseprite file; ZIP download is 30 kB. [Animated preview GIF](https://img.itch.zone/aW1nLzE4MjYxODE2LmdpZg==/original/w7%2BXCx.gif).

**License and attribution:** Creator page applies [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/), explicitly allows modification and commercial use, says credit is not required (but appreciated), and marks the work “No generative AI was used.”

**Risks:** 128×128 is much larger than the other candidates and may occupy too much screen space or make scaling artifacts more visible. “Die” may not communicate a normal development-event reaction; a smaller idle/run transition or a custom reaction policy would be needed.

### 6. [Free 16x16 Puny Character Sprites](https://merchant-shade.itch.io/16x16-puny-characters) — Shade

**Why it fits:** The free pack lists idle, walk, sword/bow/staff attacks, throw, hurt, and death. It is explicitly designed as a 16×16-style character and supplies several strong reaction states.

**Technical details:** Sprites are drawn on a 32×32 canvas but intended to fit a 16×16 space; download is `Puny-Characters.zip` (498 kB). [Animated preview GIF](https://img.itch.zone/aW1nLzkwNDUxNjkuZ2lm/original/lnhNnv.gif).

**License and attribution:** The creator page labels the pack [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/), explicitly allows commercial use and modification, says credit is optional, and marks it “No generative AI was used.”

**Risks:** User comments on the creator page report that some free-version character groups have different frame counts and alignment. Treat that as a real integration risk: choose one character, inspect its sheet, and avoid assuming every character shares one layout.

## Additional candidates and why they rank lower

- [Pixel Character 02 — James](https://opengameart.org/content/pixel-character-02-james) is a CC0 PNG spritesheet with idle, walk, attack, hit, death, and a 16×16 talking portrait. It is technically attractive, but the page does not make a no-AI disclosure and the `h16 v8` sheet notation needs interpretation before integration.
- [Adventurer and Slime game Sprites](https://opengameart.org/content/adventurer-and-slime-game-sprites) is CC0, PNG, and includes idle, attack, hurt, and dead states. The page does not state the frame dimensions or AI provenance.
- [Animated Character Base — Swordsman](https://kiddolink.itch.io/animated-character-base) supplies a PNG spritesheet, eight GIFs, and Aseprite files for idle/run/hurt/attack/jump; the creator describes it as public domain, the itch metadata labels it CC0, and the page marks no generative AI. It has no stated frame dimensions and is less companion-like than the ranked choices.
- [Pixel Boy 32x64 Animation](https://zeenaz.itch.io/free-asset-pixel-boy-32x64-animation) is CC0, explicitly 32×64, and has idle, walk, and idle-to-walk. It lacks a clearly separate reaction state, so it is better as a fallback for proving animation playback than for Phase 0 event reaction.

## Suggested next decision

Do not commit an asset from this research note yet. First open the ranked previews and select the desired visual direction. Then verify the selected archive's actual file list, animation frame counts/timing, included license/readme, and SHA-256 hash. On the evidence available without downloading, **Pixel Penguin 32x32** is the best visual/technical balance, while **Alex's 16x16 Character** is the lowest-risk runtime integration candidate.
