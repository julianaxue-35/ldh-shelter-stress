# LDH Shelter Stress Protocol

A single self-contained HTML tool implementing the LDH SOP *Shelter Stress Management – Dogs*
**v1.2** across the North Melbourne and Cranbourne sites.

Open `shelter-stress.html` in any browser. No server, no install, nothing external — it can be
emailed or dropped on a shared drive as-is.

**`index.html`** is the new **LDH Veterinary Decision Support** hub — a landing page with cards
for each species/protocol tool. It links to `shelter-stress.html` (this repo, dog) and out to the
cat gabapentin protocol in the separate `sedation-protocol` repo
(`julianaxue-35.github.io/sedation-protocol/cat_gabapentin_protocol.html`) rather than duplicating
it here. `shelter-stress.html` links back to the hub via a small `&larr;` line above its title.

**Location:** `Dropbox/work docs/job related info/LDH docs/ldh-shelter-stress-protocol/` — moved
here from `~/ldh-shelter-stress` on 13 Aug 2026 so it lives alongside the Word SOP. It is still a
git repo (`.git` moved with it) backed up to a **private** GitHub remote
(`julianaxue-35/ldh-shelter-stress`). Dropbox continuously syncs files while they're being
written, which is known to corrupt `.git` internals on an active repo — if commits here start
misbehaving, that's the likely cause; the GitHub remote is the safety net.

## Private

This repo is **internal**. It names LDH sites, Teams channel names, Sheltermate and named staff,
so it is a **private** GitHub repo with no Pages site and no public URL.

## The three modes

| Mode | For | Produces |
|---|---|---|
| **1. Start medication** | New shelter stress case | Starting dose + tablet breakdown, reassessment prompt, note for Teams/Sheltermate |
| **2. Recheck** | 48–72 h or weekly review | Maintain / increase 25% / trial the paired second-line drug / add second drug at the ceiling / refer, with the new dose and pen medication instruction |
| **3. Wean and discharge** | Adoption, foster, rescue | Weaning ladder, dispensing labels with quantities, waiver text for the Sheltermate indemnity |

## Clinical rules (SOP v1.2)

Three medications only, branching at first presentation instead of trazodone-for-everyone:

- **Fearful or shut-down dogs** start **trazodone** 5 mg/kg BID, ceiling 10 mg/kg BID (7 mg/kg BID
  on another serotonergic medication)
- **Aroused or reactive dogs** start **clonidine** 0.02 mg/kg BID, ceiling 0.05 mg/kg BID

**Partial response** (either branch): titrate the current drug up 25% each check; pregabalin
(2–10 mg/kg BID) can also be added as an adjunct.

**Ineffective response** (either branch, below the ceiling): keep titrating by 25%, or trial the
branch-paired drug immediately instead of waiting for the ceiling — fear branch trials clonidine;
arousal branch trials trazodone first, pregabalin if trazodone isn't suitable.

**At the ceiling**, add the same branch-paired second-line drug (one add-on only), then titrate it
by 25% each check up to its own ceiling. Above either ceiling, refer to the behaviour department.
Gabapentin, mirtazapine and Zylkene are not part of this pathway. A dog on trazodone at 4 weeks
should be considered for long-term behaviour medication — consult Dr Leonie Poulter for case
management.

**Serotonergic combinations.** If the dog is already on another serotonergic medication, the
trazodone ceiling drops from 10 to **7 mg/kg BID**. A shared yes/no field drives every ceiling
check in the tool, and the titration never rounds a tablet above the ceiling in force. The drug
list, serotonin syndrome signs and treatment are carried in their own section, lifted from
`~/sedation-protocol/dog-sedation.html` so the two documents agree. Note that agitation,
vocalisation, panting, mydriasis and hypersalivation appear on both the arousal chart and the
serotonin syndrome list — the tool says so, since the wrong reading leads to a dose increase.

### Stocked strengths

| Drug | Strengths | Splitting | Floor |
|---|---|---|---|
| Trazodone | 100 mg tablet | ¼, ½, ¾ | 25 mg |
| Clonidine, under 10 kg | 100 mcg, 150 mcg tablets; 1200 mcg compounded | ¼, ½, ¾ | 25 mcg |
| Clonidine, 10 kg and up | 1200 mcg compounded only | ¼, ½, ¾ | 300 mcg |
| Pregabalin | 25, 75 mg capsules | none — do not open | 25 mg |

Above 10 kg the tool stops offering the 100/150 mcg tablets — showing small-dog strengths to a
30 kg dog just causes confusion — but the 1200 mcg tablet quartered (300 mcg) can't reach the
0.02–0.05 mg/kg therapeutic range under about 10 kg, which is why the small strengths stay
available there (`DRUGS.clonidine.smallCutoffKg` in the script; see `unitsFor`/`floorFor`).
Clonidine strengths come from the Veterinary Decision Support sedation protocol
(`~/sedation-protocol/dog-sedation.html`) and **still need confirming against what LDH stocks**,
including whether 10 kg is the right cutoff.

### Weaning

28 days or less → no weaning, stop at exit. More than 28 days → reduce by 25% of the shelter dose
each week: ¾ in week 1, ½ in week 2, ¼ in week 3, stop at the end of week 4.

Each step snaps to an amount the stocked strengths can actually make. Where a capsule size forces
a step to round up, the ladder takes one extra week to reach a quarter of the shelter dose, capped
at 4 dosing weeks. A dog already on the smallest unit cannot have its dose reduced, so the
frequency halves instead: BID ×2 weeks, SID ×2 weeks, stop — this is the usual path for pregabalin
in small dogs, since its capsules cannot be opened.

**No more than two units of one strength per dose.** Six capsules twice daily is not something an
owner will manage, so any representation using two or fewer units always wins where one exists.
Where no combination reaches the dose in two units — a 60 kg dog on trazodone, which only comes as
100 mg — the tool says so rather than hiding it.

Every week is overridable from a dropdown that offers **every** way of giving that amount, so both
stepping down to a whole lower strength and cutting the tablet already in hand are available.
Labels, quantities and the waiver text all recalculate.

### Sign tick boxes

Ten signs, five arousal and five fear, chosen as the ones a shelter vet most often sees at the pen.
Fear signs point to trazodone first line, arousal signs to clonidine first line. Juliana's full
body-language reference charts (15-panel arousal, 17-panel fear, from `~/behaviour-resources/`)
appear both under the tick boxes and in *Recognising shelter stress*. The base64 is held once in a
`CHARTS` object and rendered into every `[data-charts]` container, so two placements cost one copy
of each image.

Recheck mode has its own 18-item checklist (`RC_SIGNS`) with three groups — fear, arousal (reused
from the Start-mode arousal signs) and "improved" — so a recheck can document that the dog is
settled, not just that it's still stressed. It's documentation only: ticking signs doesn't drive
the Response dropdown, which stays an explicit clinical call.

## Worked examples (regression cases)

```
Trazodone 150 mg BID (30 kg @ 5)   Clonidine 600 mcg BID (20 kg @ 0.03, >=10 kg branch)
  Wk1  1   × 100 mg = 100  67%       Wk1  ¼ × 1200 mcg = 300 mcg  50%
  Wk2  ¾   × 100 mg =  75  50%       Wk2  ¼ × 1200 mcg = 300 mcg  50%  (SID)
  Wk3  ½   × 100 mg =  50  33%       Wk3  ¼ × 1200 mcg = 300 mcg  50%  (SID)
  Wk4  ¼   × 100 mg =  25  17%       then STOP
  then STOP

Pregabalin 150 mg BID (40 kg @ 4)  Pregabalin 25 mg BID (5 kg, at floor)
  Wk1  25+75 mg = 100  67%           Wk1-2  25 mg BID
  Wk2  1 × 75   =  75  50%           Wk3-4  25 mg SID
  Wk3  2 × 25   =  50  33%           then STOP
  Wk4  1 × 25   =  25  17%
  then STOP

Clonidine 160 mcg BID (8 kg @ 0.02, <10 kg branch)
  Wk1  ¾ × 150 mcg = 110 mcg  69%
  Wk2  ½ × 150 mcg =  80 mcg  50%
  Wk3  ¼ × 150 mcg =  40 mcg  25%
  then STOP
```

## Relationship to the Word SOP

The parent folder (`Dropbox/.../LDH docs/`) holds `Shelter Stress Protocol v1.2.docx`, which
carries the same changes, written into the original SOP with its sentence shapes preserved and a
v1.2 row added to the version-control table. `Shelter Stress Protocol.docx` (v1.0) sits alongside
it, untouched. The tool's *What changed* section lists the differences from v1.0 through v1.2 for
staff who learned an earlier protocol.

The SOP's section 5 link points to this folder's `shelter-stress.html` by local file path — it
only resolves on this machine. If the SOP is shared more widely, that link and the file both need
to move somewhere every reader can reach.

## Testing

The dosing engine is pure and testable. With `jsdom` installed:

```bash
node --check <(sed -n '/<script>/,/<\/script>/p' shelter-stress.html | sed '1d;$d')
```

Regression cases live in the worked examples above — check each still produces the printed ladder
after any change to `doseLadder`, `pickNearest` or `optionCost`.

---

© 2026 Juliana Xue. All rights reserved. This work is the intellectual property of Juliana Xue and
may not be copied, modified, distributed or used without her express written permission, except by
her current employer for as long as she remains engaged there.
