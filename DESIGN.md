# Design & Roadmap

## Concept: Bharat — the independent civilizational thread, Netaji's vision

A custom **Contemporary Era Culture** for Humankind representing Bharat as
Netaji Subhas Chandra Bose envisioned it: the self-reliant, composite,
independent nation-state, not the generic modern India already in the base
game. Same rationale as the shipped Civ VII mod
([civ7-netaji-mod](https://github.com/ovhirup/civ7-netaji-mod)) — the base
game already has Indian content (Mauryans in Classical, Mughals in Early
Modern, a generic "Indians" culture in Contemporary), so this is not a
gap-fill. It's a thematically distinct culture: composite nationalism and
planned self-reliance born from the independence struggle, not empire or a
default modern nation-state.

Ancestral thread through eras already in the base game — thematically
referenced, not (yet confirmed) mechanically gated: **Mauryans** (Classical)
→ **Mughals** (Early Modern, geographic/imperial pairing, same "demoted to
one option among several" role Mughal played in the Civ VII arc) →
**Bharat** (Contemporary, the custom culture, the nation built *after and
against* empire).

## Non-negotiable framing rule (carried over from civ7-netaji-mod, extended)

1. **No Axis-alliance references anywhere** — code, text, or art. Same rule,
   zero exceptions.
2. **Every historical claim is tiered** — reused directly from the Civ VII
   research discipline:
   - **Tier A** — established consensus (cite it).
   - **Tier B** — debated/contested (say what's agreed vs. disputed).
   - **Tier C** — mythological/literary/cultural heritage — real *as
     culture*, never presented as literal history or science.
   - **Tier D** — debunked-as-fact — **never used** as a real claim. If
     there's a genuine kernel underneath (e.g. Panini's formal grammar under
     the false "Sanskrit invented quantum computing" claim), that kernel is
     what gets used, not the debunked claim.
3. **New tier for this project — Tier E: alternate history, explicitly
   labeled.** This is the resolution to the ambition of this mod (advanced
   ancient tech, world supremacy in spirituality/theology, a computing
   lineage from ancient mathematics, naval dominance): built from real Tier
   A/B/C anchors, extended into an explicit **"what if uninterrupted"**
   speculative timeline — but every place it appears in-game (flavor text,
   Culture description, unique building/unit blurbs) **must read as
   deliberate alternate-history framing**, never as an assertion of real
   history. This is the choice made over a "maximalist myth-as-fact" version
   specifically to preserve the credibility the Civ VII mod earned. Decided
   2026-07-18.
4. **Reject Vimana/Vaimanika Shastra as literal ancient aircraft, and any
   other Tier D claim treated as fact** — same rejection as civ7-netaji-mod,
   for the same reason (early-1900s text, not ancient; a credibility
   liability). Vimanas may appear as Tier C myth/culture flavor only.

## Reused directly from civ7-netaji-mod's research (no need to re-derive)

- **Bose's actual program:** convened the National Planning Committee in
  1938 as Congress president (Nehru chaired it — always "convened," never
  "chaired") → the real anchor for a self-reliance/planned-industry
  mechanic ("Mother Industries" in the Civ VII mod).
- **INA / Azad Hind Fauj:** deliberately composite Hindu-Muslim-Sikh
  regiments, "Jai Hind," "Dilli Chalo" — the unity theme, not religious
  identity.
- **Historical anchors already vetted at Tier A/B:** pre-colonial economic
  share (~¼ world GDP) and British-era deindustrialization/"drain of
  wealth" (Naoroji); Chola naval expeditions (Rajendra Chola) and the
  Maratha navy (Kanhoji Angre); Sushruta's surgery; Kautilya's Arthashastra;
  Nalanda/Takshashila; Aryabhata/Brahmagupta/Bhaskara and the zero/decimal
  system; wootz steel and the Delhi Iron Pillar's corrosion resistance;
  Panini's Ashtadhyayi as a formal/generative grammar with documented,
  real interest to computational linguistics — the *correct* alternative to
  the false "Sanskrit invented quantum computing" claim.
- **Tier C heritage:** Ramayana/Mahabharata, temple/monument achievements
  (Konark etc.) — myth and monument as culture, never literal suppressed
  tech.

## What's new for this mod (the alternate-history layer, Tier E)

Extend the real anchors above forward, explicitly labeled as speculative:
- **Naval supremacy:** Chola/Maratha maritime power, in this timeline,
  never interrupted by colonization — grows into sustained Indian Ocean/
  global naval dominance. (Real anchor: Tier A/B naval history above.)
- **Spiritual/philosophical global influence:** India's genuine, real
  export of yoga, Vedanta, and Buddhist philosophy, extended as a
  speculative "what if this soft-power thread had continued unbroken and
  scaled globally" — a culture/influence-victory fantasy, not a claim about
  real theology's actual global status.
- **A computing lineage from Panini:** the real, documented interest of
  Panini's formal grammar to computer science and knowledge representation,
  extended speculatively into an alt-history "unbroken lineage into a
  distinctly Indian computing/information-theory tradition" — explicitly
  NOT a real quantum-computing claim, and the copy must say so or imply it
  clearly through "in this timeline" framing.
- **Advanced ancient technology more broadly:** framed as "what if the
  Nalanda/Takshashila scientific tradition, wootz-steel metallurgy, and
  Aryabhata's mathematics had continued accelerating instead of being
  disrupted" — an extrapolation of real achievements, not invented artifacts
  presented as literally excavated.

## Structural gap vs. civ7-netaji-mod (open, needs hands-on verification)

Humankind has **no persistent "leader" or fixed "civilization"** the way
Civ VII does — an Avatar (empire) picks a new **Culture** each era, each
with an **Affinity** (Militarist/Aesthete/Builder/Merchant/Scientist/
Expansionist/Ecologist — name list needs hands-on confirmation), a unique
unit, an emblematic district/quarter, and a Legacy trait. This means:

- **Netaji/Bose as a named "leader" may not have a direct equivalent** — the
  game's avatar/persona system is not yet verified hands-on. Needs checking
  once tooling allows.
- **The Maurya → Mughal → Bharat "thread"** is thematic/flavor-text framing
  by default; Humankind may or may not have a mechanism for
  prior-culture-choice bonuses (Civ VII's literal age-transition unlock
  rows have no confirmed Humankind analog yet). Verify before promising
  this as a mechanic rather than narrative framing.
- **Biggest open risk: the tooling gap.** The official Mod Tools (Unity
  Editor, Import-from-Archives, Descriptors/Definitions/UIMappers) are
  Windows-only and unavailable on this Mac (see GROUND-TRUTH.md). Only
  BepInEx (runtime C#/Harmony patching) currently works here. **Authoring a
  wholly new Culture from scratch may not be feasible through BepInEx
  alone** — that's normally a data-authoring job the Mod Editor GUI does
  against the binary Mercury databases. A more BepInEx-tractable scope may
  be: **reskinning/extending the existing "Indians" Contemporary culture**
  (rename, re-theme, adjust its unique unit/district/Affinity via runtime
  patches) rather than adding a brand-new Culture row. **This needs
  hands-on research before the build stages below are locked in** — it may
  change the technical approach even if the creative content above stays
  the same.

## Build stages (draft — contingent on the tooling-gap research above)

1. **Toolchain resolution** — determine whether BepInEx can add a new
   Culture, or whether the realistic path is patching the existing
   "Indians" culture. This gates everything else.
2. **Loadable skeleton** — Bharat (or "Bharat-reskinned-Indians") appears
   as a selectable Contemporary Era culture, loads clean, no crash.
3. **Identity** — Affinity assignment, Legacy trait (self-reliance +
   composite-unity themed, adapting "Mother Industries" from the Civ VII
   mod).
4. **Unique unit** — Azad Hind Fauj infantry (direct reuse of the Civ VII
   unit concept).
5. **Unique district/emblematic quarter** — National Planning Commission
   (Yojana Bhavan + Vaidyashala pairing, reused from the Civ VII mod).
6. **Text pass** — Culture description, unit/district blurbs, and the
   Tier E alternate-history flavor text, written to the framing rule above.
7. **Balance, art, ship** — same shape as civ7-netaji-mod's later stages.

## Resolved: BepInEx cannot author a new Culture

Per Amplitude's official Modding Guide (§10, read in full 2026-07-18 —
details in GROUND-TRUTH.md), a new Culture is authored as a Unity
serialized object graph (`FactionDefinition` + `FactionUIMapper` +
`LegacyTrait` + `LegacyTraitDescriptor`, cross-referenced by object
pickers) entirely through the Mod Editor's GUI — there is no hand-editable
text file format. BepInEx (runtime C#/Harmony patching) is the wrong tool
for constructing new content this way. **This is a real fork in the road:**

- **Option A — Windows VM (Parallels/UTM):** run the actual Mod Editor,
  build Bharat as a genuinely new Culture per the official workflow above.
  Full ambition, but real setup cost/overhead.
- **Option B — Reskin the existing "Indians" culture via BepInEx:** modify
  an already-loaded object's fields/text at runtime instead of constructing
  a new object graph — smaller, BepInEx-tractable scope, but a
  ceiling-limited "Bharat re-theme" rather than a from-scratch culture.

Not yet decided — see the open question below.

## Open questions (blocking, need a decision or hands-on confirmation before locking scope)

- [ ] **Decision needed:** Option A (Windows VM, full new Culture) vs.
      Option B (BepInEx reskin of "Indians," smaller scope) — see above.
- [ ] Does Humankind have any named-persona/leader system at all, or is the
      Avatar always player-customized with no historical-figure equivalent?
- [ ] Confirm the real Affinity list and pick the best fit for Bose's
      military-organizer + coalition-builder + planner identity (Civ VII
      used Militaristic + Diplomatic; Humankind's closest analogs need
      checking).
- [ ] Is there a Chola culture in the base game already (search results
      didn't surface one) — matters for the ancestral-thread framing.
