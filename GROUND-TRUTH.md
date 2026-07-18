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

**Confirmed Windows-only** (checked the actual Steam Tools store listing on
this Mac, 2026-07-18 — "Available for" shows the Windows icon only, no Mac
build offered). This Mac is Apple Silicon, so this path is closed unless run
inside a Windows VM (Parallels/UTM) — **not pursued for now**; BepInEx below
is the active path instead.

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
own community folder." — the actual Mac-side folder was found by direct
filesystem inspection (2026-07-18):
`~/Library/Application Support/Humankind/Community/` (currently containing
only a `Scenarios` subfolder). Not `~/Documents/...` as the Windows-phrased
guidance suggested — Mac app-support data lives under `Library/Application
Support`, not `Documents`, consistent with the other confirmed paths below.
Still needs a real mod build dropped in to confirm this is where a
Mod-Tools-exported project would actually land (this folder may just be
Scenario/Map-Editor output, a separate feature from gameplay-data mods).

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

### Mac install steps (community-documented, not yet hands-on verified)

Since the official Mod Tools are Windows-only, BepInEx is now the **active**
path on this Mac. Sources: [BepInEx Mono/Unity install
docs](https://docs.bepinex.dev/master/articles/user_guide/installation/unity_mono.html),
[Amplitude forum BepInEx
thread](https://community.amplitude-studios.com/amplitude-studios/humankind/forums/213-game-resources/threads/46128-tutorials-showing-how-to-install-bepinex-mods-windows-and-mac),
a [Steam Community
report](https://steamcommunity.com/app/1124300/discussions/0/3183486955460515102/)
of a real Mac install. Steps:

1. Download the `Unity.Mono-macos-x64` BepInEx build (64-bit only supported
   on Mac).
2. Unpack into the game root — same folder as `Humankind.app`, i.e.
   `~/Library/Application Support/Steam/steamapps/common/Humankind/` on this
   Mac.
3. Edit the included `run_bepinex.sh`: set
   `executable_name="Humankind.app"`, then `chmod u+x run_bepinex.sh`.
4. In Steam, set Humankind's launch options to:
   `"/Users/abhirupbanerjee/Library/Application Support/Steam/steamapps/common/Humankind/run_bepinex.sh" %command%`
5. Launch normally through Steam (not the script directly) — first run
   generates the BepInEx config folder and log file.

**Known conflict:** Steam Overlay and BepInEx both use OS-level injection on
Mac and interfere with each other — **Steam cloud snapshots/character
creator break while BepInEx is active** (confirmed by a real user report, not
just theoretical). Workaround in use by the community: create characters via
BepInEx mod personas, then disable BepInEx to restore snapshot functionality.
Combine this with the already-noted `Modding.Humankind.DevTools` +
`AOM.Humankind.Teams` crash conflict — check compatibility before stacking
BepInEx mods.

### Hands-on result on this Mac (2026-07-18): install works, but game is currently unplayable with BepInEx active

Followed the install steps above exactly (BepInEx 5.4.23.5, `macos_universal`
build). Findings, in order encountered:

1. **Preloader crash #1** — `ConsoleSetOutFix.Apply()` threw
   `NullReferenceException` in `MonoMod.RuntimeDetour.DetourHelper
   .GetIdentifiable`, an IL-hook/detour failure. This blocks the game at the
   OS process level (game process alive but never renders, CPU flat) before
   any actual game code runs.
   - Tried the documented fix for this exact error class (`HarmonyBackend =
     cecil` in `BepInEx/config/BepInEx.cfg`, per [BepInEx troubleshooting
     docs](https://docs.bepinex.dev/articles/user_guide/troubleshooting.html))
     — **did not help**. This setting only affects Harmony's *method-patch*
     compilation strategy; `ConsoleSetOutFix` uses a raw `MonoMod.RuntimeDetour
     .ILHook` directly, a different code path the setting doesn't reach.
2. **Preloader crash #2** — with `ApplyRuntimePatches = false` (disables
   *all* preloader bootstrap fixes, config's own documented escape hatch for
   "can't start the game due to a Harmony related issue"), the
   `ConsoleSetOutFix` crash is skipped entirely and the chainloader proceeds
   further (creates `BepInEx/cache/`) — but a **second, different** Harmony
   patch (`HarmonyInteropFix.Apply()`, patching
   `System.Reflection.Assembly::LoadFile`) throws the same
   `DetourHelper.GetIdentifiable` `NullReferenceException`. Non-fatal this
   time (game continues past it), but confirms this MonoMod ILHook detour
   mechanism is broadly broken against this game's bundled Mono runtime, not
   just one fix.
3. **Game now loads further but hangs at a different point** —
   `~/Library/Logs/AMPLITUDE Studios/Humankind/Player.log` shows it reaching
   real UI code (`JoinGameScreen`), then throwing `Exception: Failed to
   initialize the freetype library` (font rendering) — no text can render,
   so the loading screen appears permanently stuck.
4. **Isolated the cause with a vanilla-launch A/B test**: cleared Steam's
   launch options (removing the `run_bepinex.sh` wrapper) and launched
   Humankind completely unmodified. **Result: loads cleanly past the exact
   same point** — no freetype exception, no `DOORSTOP_*` env vars on the
   process (confirmed via `ps eww`), log shows normal asset/font/Photon
   networking loading straight through to the main menu.

**Conclusion (superseded — see resolution below): BepInEx's Doorstop
injection itself — not any specific plugin, and not a pre-existing Mac-port
bug — breaks this game's native `freetype6.dylib` loading on this Mac.**

### Resolution: it's a version regression in 5.4.23.5, not BepInEx itself

Swapped to **v5.4.23.2** (`BepInEx_macos_x64_5.4.23.2.zip`) — the oldest
release with a dedicated macOS build (mac support was split out of the
combined unix zip starting at this exact version) — as a clean install (old
`BepInEx/`, `run_bepinex.sh`, `libdoorstop.dylib`, `.doorstop_version`
removed first, no carried-over config). **Result: works cleanly.**
`BepInEx/LogOutput.log` shows a normal startup (`Preloader finished` →
`Chainloader started` → `0 plugins to load` → `Chainloader startup
complete`), zero freetype exceptions in `Player.log` (down from 5
occurrences per run on 5.4.23.5), confirmed via `ps eww` that
`DYLD_INSERT_LIBRARIES=libdoorstop.dylib` and `DOORSTOP_ENABLED=1` were
genuinely active (so this is a real BepInEx-active success, not an
accidental vanilla launch).

**Verdict: v5.4.23.5's bundled MonoMod/Doorstop build has a regression that
breaks font-library loading specifically on macOS with this game;
v5.4.23.2 does not have this regression. Use v5.4.23.2 (or test v5.4.23.3/
5.4.23.4 later if a newer feature is ever needed) for this project, not the
latest release.** Untested so far: v5.4.23.3 and v5.4.23.4 (the two versions
between the working and broken ones) — not needed unless 5.4.23.2 turns out
to be missing something required later.

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

- [x] ~~Install "Humankind Mod Tools" from Steam Library → Tools~~ — **not
      possible**: confirmed Windows-only on this Apple Silicon Mac
      (2026-07-18). Official Mod Editor path is closed unless run in a
      Windows VM; not pursued for now.
- [x] Install BepInEx via the Mac steps above — **done and working**, using
      **v5.4.23.2** specifically (v5.4.23.5 has a font-loading regression on
      Mac, see hands-on results above).
- [ ] Write an actual first plugin (even a trivial "hello world" log
      message) and confirm `BepInEx/plugins/` + chainloader picks it up —
      still unverified that a *real* plugin loads cleanly, only that 0
      plugins loads cleanly.
- [ ] Test whether the Steam-Overlay-vs-BepInEx snapshot conflict actually
      reproduces on this install, and whether the mod-persona workaround is
      needed.
- [ ] Since the Unity-Editor "Import from Archives" categories
      (LandUnit/Descriptors/Definitions/UIMappers) are no longer directly
      reachable, figure out whether BepInEx-based mods can read/patch these
      same Mercury database concepts, or whether BepInEx modding is a
      fundamentally different, lower-level surface (Harmony IL patches, not
      data-driven Definitions).
- [ ] Confirm whether `~/Library/Application Support/Humankind/Community/`
      is actually where a gameplay mod (not just Scenario/Map-Editor output)
      would land.
- [ ] Check mod.io's Humankind mod listing for any large, well-documented
      mods worth studying as precedent (the same way civ7-netaji-mod studied
      12 installed Workshop mods for wiring patterns) — specifically look for
      ones built via BepInEx rather than the official Mod Editor, since
      that's now the only viable path on this Mac.
