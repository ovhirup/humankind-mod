# Humankind Mod (project scaffold)

**Reminder before anything else:** everything under "Ground truth" below is
second-hand research (official blog posts + one community mod's README),
gathered 2026-07-18 — **not yet hands-on verified**. Before building real
content, install "Humankind Mod Tools" from Steam Library → Tools, launch it
once, and confirm these claims against the actual Mod Editor. Same rule the
Civ VII project lived by: never trust community docs over what you can check
yourself. (This reminder repeats at the bottom — see "Sandwiching" below for why.)

Mod project for **Humankind** (Amplitude Studios / SEGA, Steam appid
`1124300`), built on **Unity**. No specific mod concept has been chosen yet —
this file is the reusable meta-prompt + working methodology for whatever gets
built here. Fuller research: [GROUND-TRUTH.md](GROUND-TRUTH.md). Roadmap once
a concept exists: [DESIGN.md](DESIGN.md).

## Ground truth (verify hands-on before trusting)

- **Engine:** Unity (`MonoBleedingEdge` + `AssetBundles` confirmed on disk).
  Internal data codename **"Mercury"** (`MercuryDatabases` asset bundle, a
  packed Unity binary — not directly human-readable like Civ VII's XML).
- **Official mod path:** "Humankind Mod Tools" (Steam Library → Tools
  category) — bundles Unity Hub + the real Unity Editor. Workflow: Mod
  Editor GUI → **"Import from Archives"** (pulls existing game databases) →
  edit → Build → export straight to mod.io from inside the tool.
- **Three data concepts** in the Mod Editor (Amplitude's own vocabulary):
  - **Descriptors** — an effect/tag applicable to a game element (closest
    analog to Civ VII's `GameEffects` Modifiers).
  - **Definitions** — game elements + which Descriptors apply to them
    (closest analog to Civ VII's base entity rows + trait-attach tables).
  - **UIMappers** — UI/localization mapping data for an element.
- **Local mod-project folder** (community-shared; exact Mac path
  unverified): `Documents/Humankind/Community/<mod-name>/` — official
  guidance is literally "send the folder to your friends."
- **Distribution:** mod.io is Amplitude's official partner (not Steam
  Workshop), browsable/subscribable from inside the game. Steam Workshop
  isn't dead though — confirmed locally: a real subscribed `.hmap` (map)
  file exists under `steamapps/workshop/content/1124300/`. So Workshop
  carries at least map/scenario content; mod.io is the promoted channel
  for gameplay mods.
- **A second, unofficial, deeper path exists: BepInEx** (Harmony-style .NET
  assembly patching). Community devs decompiled Humankind's own assemblies
  and found 100+ built-in-but-hidden dev tools (profilers, Map Editor,
  District Painter, World Generation, AI/battle debug panels). **Known
  conflict:** a BepInEx devtools-unlock mod crashes the game alongside at
  least one other mod (`AOM.Humankind.Teams`) — check compatibility before
  stacking BepInEx mods. Reference:
  github.com/Theadd/Humankind-GUI-Tools.
- **This Mac's paths:** install at `~/Library/Application Support/Steam/
  steamapps/common/Humankind/`; app-support data at `~/Library/Application
  Support/Humankind/` and `~/Library/Application Support/
  com.Amplitude.Humankind/` (diagnostics/temp only so far — no user content).

## Which path to use

Default to the **official Mod Tools** (Unity Editor + Descriptors/
Definitions/UIMappers) for "add or change gameplay content" — same
principle as the Civ VII project's rule to prefer the sanctioned template
over reverse-engineering. Reach for **BepInEx** only for a capability the
Mod Editor genuinely doesn't expose (mirrors Civ VII's UI-JS wrapping, which
was only needed once the data-only path hit a real wall). Never assume a
capability doesn't exist without checking hands-on first.

## Working methodology (carry these over — they were proven on civ7-netaji-mod)

### Sandwiching
State the single most important instruction/constraint at the **start** of
any long prompt, research brief, or Agent/Workflow dispatch, **and restate a
condensed version at the end**. LLM attention is strongest at the beginning
and end of a context window and weakest in the middle ("lost in the
middle") — a rule stated once in the middle of a long document is the most
likely thing to get silently dropped. Apply this to: every research brief
sent to an external Advisor, every Agent/Workflow prompt with a required
output format, and this file itself (note the reminder repeats below).

### Loop Engineering
The reusable dev loop, proven across the whole Civ VII project:
1. **Research** — Advisor tier gathers candidate design + external
   knowledge (WebSearch/WebFetch, or an external chat project).
2. **Verify** — ground-truth check against the *actual* installed
   game/tool. Never trust the research's technical claims un-checked;
   dispatch a background Agent/Workflow to keep raw exploration out of
   the main session.
3. **Implement** — Executioner tier writes the actual mod content,
   following only verified patterns.
4. **Validate** — does it build clean in the Mod Editor / load clean
   in-game, no errors.
5. **Test in-game** — the user drives this; only they have eyes on the
   running game.
6. **Commit** — with a message that records the *lesson*, not just the
   diff.
7. Loop back to 1 for the next feature.

Use the `/loop` skill for literal recurring checks (e.g. periodically
re-verifying mod.io/tooling hasn't changed), and the Workflow tool's
loop-until-dry / adversarial-verify patterns for exhaustive one-time
research passes (e.g. a full sweep of every Descriptor category before
committing to a mod's design) — mirrors the 4-agent schema sweep and the
tiered heritage-research pass done for Bharat.

### Top model = Advisor, cheaper/faster model = Executioner
Reserve the current high-capability model (high effort) for: ground-truth
verification, ambiguous judgment calls (balance, thematic framing),
debugging genuinely mysterious failures, and the final adversarial review
before shipping. Delegate to a cheaper/faster model (via the Agent tool's
`model` param, or a Workflow agent's `model`/`effort` override) for:
mechanical reconnaissance ("list every Descriptor category in the
Import-from-Archives window, verbatim") and repetitive data-entry once a
pattern is established from one verified example ("apply this same
Definition pattern to these 12 units"). Same
Haiku-recon/Sonnet-execute/Fable-verify split already validated on
civ7-netaji-mod.

### Token optimization
- Dispatch heavy exploration/file-reading to background Agents/Workflows —
  only the distilled verdict returns to the main session, never raw dumps.
- Persist hard-won facts to the auto-memory system between sessions instead
  of re-deriving them each time (this file + GROUND-TRUTH.md are the seed —
  keep them current as real facts get confirmed).
- Grep/targeted-search before whole-file reads; never re-read a file
  already read this session.
- Batch independent tool calls in parallel by default.
- Low effort for Executioner-tier calls; reserve high/xhigh for
  Advisor-tier judgment calls and verification.
- Use the Workflow tool's `budget` mechanism to cap spend on large
  fan-outs when a token target is given.

## Conventions

Concept chosen 2026-07-18: **Bharat** — a custom Contemporary Era Culture for
Netaji Subhas Chandra Bose's independence-era vision, adapted from the
shipped Civ VII mod (civ7-netaji-mod). Full concept, reused research, and
open questions: [DESIGN.md](DESIGN.md).

- **Non-negotiable framing rule:** no Axis-alliance references anywhere —
  code, text, or art. Every historical claim is tiered A (established) / B
  (debated) / C (mythology-as-culture) / D (debunked, never used as fact) /
  **E (alternate history, explicitly labeled as speculative "what if,"
  never presented as real history)** — this last tier is how this mod
  reaches for ancient-tech/spirituality/naval "world supremacy" ambition
  without losing the credibility the Civ VII mod earned. Reject
  Vimana/Vaimanika Shastra as literal ancient aircraft (Tier D) — myth-only
  (Tier C) at most.
- Type-name / id prefix convention: TBD — blocked on resolving whether
  BepInEx can author a new Culture at all, or only patch an existing one
  (see DESIGN.md's open questions).
- Repo layout: TBD once the Mod Tools' own project structure is seen
  hands-on — it likely dictates a required folder shape, unlike Civ VII's
  free-form XML tree. Note: the official Mod Tools are confirmed
  Windows-only and unavailable on this Mac (see GROUND-TRUTH.md) — BepInEx
  is the active toolchain, which changes what "project structure" even
  means here.

## Division of labor

Same split as civ7-netaji-mod: implementation, ground-truth verification,
and build/debug work happens here in Claude Code; creative writing,
external research, and thematic design happen in an external Advisor chat
project (Grok or similar), using the `grok-project/` templates in this repo.

---
**Reminder (repeated from the top):** the "Ground truth" section above is
unverified second-hand research until someone opens the real Mod Tools by
hand — check before building on it.
