# Ground truth — Humankind modding research (2026-07-18)

Detailed backing for [CLAUDE.md](CLAUDE.md)'s summary. Status: **second-hand,
unverified hands-on.** Update this file with real facts (file paths, actual
category names, screenshots of the Mod Editor) as they're confirmed, the same
way civ7-netaji-mod's memory accumulated hard-won schema facts over its build.

## Sources consulted

- [Modding forum index](https://community.amplitude-studios.com/amplitude-studios/humankind/forums/178-modding) — Amplitude's official modding forum.
- [Modding Tools Beta Basics](https://community.amplitude-studios.com/amplitude-studios/humankind/blogs/788-modding-tools-beta-basics) — official walkthrough of the Mod Editor.
- [Mod Tools Beta Available Now](https://community.amplitude-studios.com/amplitude-studios/humankind/blogs/790-mod-tools-beta-available-now) — announcement post.
- [Humankind Mods on mod.io](https://mod.io/g/humankind) — the official distribution hub.
- [Theadd/Humankind-GUI-Tools](https://github.com/Theadd/Humankind-GUI-Tools) — a community BepInEx mod; its README describes decompiling Humankind's own assemblies.
- Direct filesystem inspection of this Mac's install (`steamapps/common/Humankind/`) and Steam Workshop cache.

## What the official Mod Tools actually do (per the Beta Basics post)

> "To create a new mod for Humankind, you should run the Humankind Mod Tools
> you can install through your Steam library (in the Tools category)."

The tool bundles Unity Hub + Unity Editor and walks a first-time user through
installing both plus activating a free Unity license. Modders don't start
from a blank Unity project — they use the Mod Editor's **"Import from
Archives"** function, which surfaces the *existing* game's own databases,
organized by category. The post names these categories: **LandUnit**,
**Descriptors**, **Definitions**, **UIMappers** (this list is very likely not
exhaustive — the actual Mod Editor probably has many more categories; this is
just what the walkthrough post happened to show).

Three data concepts, in the tool's own words:
- **Descriptors** — "define an effect in game that can be applied to a game
  element, either changing a value or working as a 'tag'."
- **Definitions** — "describe game elements and which descriptors apply to
  them."
- **UIMappers** — "define user interface data for such elements, like the
  reference key for the right localization entry."

Workflow: import → edit in the Unity Editor's GUI → **Build** (or "Build and
Run") → **Export** directly to mod.io from inside the tool (one-click, after
logging into a mod.io account in the Export tab).

Mod projects are folder-based: "Just send the mod's folder in
`Documents/Humankind/Community` to your friends so they can put it in their
own community folder." — **the exact Mac-side path for this is unverified**;
Windows convention would be `Documents\Humankind\Community\`; Mac is likely
`~/Documents/Humankind/Community/` but this needs hands-on confirmation once
the Mod Tools are actually installed and run once.

## The unofficial deeper path: BepInEx

`Theadd/Humankind-GUI-Tools`'s README states the mod requires **BepInEx**
installed in the game directory — BepInEx is a general-purpose Unity mod
loader that patches the game's .NET/Mono assemblies at runtime (Harmony-style
IL patching), a fundamentally different and more powerful mechanism than the
sanctioned Mod Editor's data-import/export flow.

The author states they **decompiled the Humankind assemblies back to
source**, and discovered 100+ hidden developer/debug tools Amplitude built in
but never exposed publicly: profiling windows (Memory, Graphics, GPU), debug
utilities (AI, Battle, Network), and development tools (Map Editor, District
Painter, World Generation). This means Amplitude's own internal dev tools are
present in the shipped game binary, just gated off — a BepInEx mod can
re-expose them.

**Known compatibility issue:** "the Modding.Humankind.DevTools library used
alongside the AOM.Humankind.Teams mod caused game crashes on startup" — check
for conflicts before stacking BepInEx-based mods with each other.

The README does **not** specify actual assembly names, file paths, or
concrete data formats beyond this — those remain to be discovered hands-on if
this path is ever needed.

## Distribution: mod.io vs. Steam Workshop

Amplitude's official messaging (per WebSearch results): "Amplitude has
partnered with Mod.io as their official partner for Humankind mods... you can
browse for new mods from within the game itself." Mods are subscribed to and
downloaded via mod.io integration built into the game's own Mod screen.

However, direct inspection of this Mac's Steam library shows a **real,
locally-cached Steam Workshop item** for Humankind (appid `1124300`):

```
steamapps/workshop/content/1124300/2965192581/
  "Huge Earth for Humankind - All DLC Natural Wonders.hmap"
```

A `.hmap` file — almost certainly Humankind's built-in **Map/Scenario
Editor** export format (a feature of the base game itself, likely not
requiring the Unity Mod Tools at all for simple maps). This confirms Steam
Workshop is at minimum a live channel for **map/scenario** content, even if
mod.io is the promoted channel for gameplay-data mods. Worth checking whether
gameplay mods (not just maps) also appear on Steam Workshop before assuming
mod.io is the only place to look for precedent.

## Filesystem facts confirmed directly (this Mac)

```
Steam appid:        1124300
Install root:        ~/Library/Application Support/Steam/steamapps/common/Humankind/
  ├── Humankind.app/Contents/          — the Unity player app bundle
  │     ├── MonoBleedingEdge/          — Mono runtime (Unity's scripting backend)
  │     ├── Resources/Data/            — standard Unity "_Data" folder equivalent
  │     ├── Frameworks/
  │     │     ├── ZFGameBrowser.app    — likely the in-game server browser (Steamworks P2P?)
  │     │     └── Chromium Embedded Framework.framework  — UI is likely CEF-rendered
  │     └── PlugIns/AkSoundEngine.bundle  — Wwise audio (same middleware Civ VII uses)
  ├── AssetBundles/                    — Unity asset bundles, PLAIN FOLDER outside the app bundle
  │     ├── MercuryDatabases/          — packed gameplay-data asset bundle (binary, ~21MB)
  │     ├── Mercury.Data.*             — other Mercury-prefixed data bundles (Units, UI, Audio, etc.)
  │     └── ...
  ├── Registry.xml                     — app settings only (display mode, Steamworks timeouts) — NOT mod-relevant data
  ├── Poster/, Soundtrack/             — marketing assets, not mod-relevant
Steam Workshop cache: ~/Library/Application Support/Steam/steamapps/workshop/content/1124300/
App-support data:     ~/Library/Application Support/Humankind/ (Diagnostics.html, Temporary Files — nothing else yet)
                      ~/Library/Application Support/com.Amplitude.Humankind/
```

**Notable parallel to Civ VII:** the UI likely runs through Chromium Embedded
Framework (CEF) — conceptually similar to Civ VII's cohtml UI-JS layer. If a
mod ever needs UI-level hooking beyond what the Mod Editor exposes, CEF-based
UI is a plausible seam to investigate (unverified — just a structural
observation, not a proven capability).

## Open questions for the first hands-on session

- [ ] Install "Humankind Mod Tools" from Steam Library → Tools; confirm it
      launches, walks through Unity Hub/Editor setup as described.
- [ ] Open the Mod Editor, click "Import from Archives," and get the **real,
      complete** list of database categories (LandUnit/Descriptors/
      Definitions/UIMappers is likely a partial list from the walkthrough
      post, not exhaustive).
- [ ] Confirm the actual Mac path for `Documents/Humankind/Community/`.
- [ ] Try Build + Export on an untouched import, to see the exact mod folder
      structure it produces (this becomes the real repo layout convention).
- [ ] Check mod.io's Humankind mod listing for any large, well-documented
      mods worth studying as precedent (the same way civ7-netaji-mod studied
      12 installed Workshop mods for wiring patterns).
