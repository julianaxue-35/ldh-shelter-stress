# LDH Shelter Stress Protocol

A single self-contained HTML tool implementing the LDH SOP *Shelter Stress Management – Dogs*
**v1.1** across the North Melbourne and Cranbourne sites.

Open `shelter-stress.html` in any browser. No server, no install, nothing external — it can be
emailed or dropped on a shared drive as-is.

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
| **2. Recheck** | 48–72 h or weekly review | Maintain / increase 25% / add second drug / refer, with the new dose and pen medication instruction |
| **3. Wean and discharge** | Adoption, foster, rescue | Weaning ladder, dispensing labels with quantities, waiver text for the Sheltermate indemnity |

## Clinical rules (SOP v1.1)

Three medications only. Trazodone 5 mg/kg BID first line for **every** dog regardless of size,
titrated in 25% increments to a ceiling of 10 mg/kg BID. At the ceiling, add ONE second-line drug
chosen on the presenting signs:

- **Clonidine** 0.02 mg/kg BID — aroused or reactive dogs, ceiling 0.05 mg/kg BID
- **Pregabalin** 2–10 mg/kg BID — fearful or shut-down dogs, ceiling 10 mg/kg BID

Above either ceiling, refer to the behaviour department. Gabapentin, mirtazapine and Zylkene are
not part of this pathway. A dog on trazodone at 4 weeks should be considered for long-term
behaviour medication — consult Dr Leonie Poulter for case management.

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
| Clonidine | 100 mcg, 150 mcg tablets; 1200 mcg compounded | ¼, ½, ¾ | 25 mcg |
| Pregabalin | 25, 75 mg capsules | none — do not open | 25 mg |

Clonidine strengths come from the Veterinary Decision Support sedation protocol
(`~/sedation-protocol/dog-sedation.html`) and **still need confirming against what LDH stocks**.

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
Arousal signs point to clonidine, fear signs to pregabalin. Juliana's full body-language reference
charts (15-panel arousal, 17-panel fear, from `~/behaviour-resources/`) appear both under the tick
boxes and in *Recognising shelter stress*. The base64 is held once in a `CHARTS` object and
rendered into every `[data-charts]` container, so two placements cost one copy of each image.

## Worked examples (regression cases)

```
Trazodone 150 mg BID (30 kg @ 5)   Clonidine 400 mcg BID (20 kg @ 0.02)
  Wk1  1   × 100 mg = 100  67%       Wk1  2 × 150 mcg = 300  75%
  Wk2  ¾   × 100 mg =  75  50%       Wk2  2 × 100 mcg = 200  50%
  Wk3  ½   × 100 mg =  50  33%       Wk3  1 × 100 mcg = 100  25%
  Wk4  ¼   × 100 mg =  25  17%       then STOP
  then STOP

Pregabalin 150 mg BID (40 kg @ 4)  Pregabalin 25 mg BID (5 kg, at floor)
  Wk1  25+75 mg = 100  67%           Wk1-2  25 mg BID
  Wk2  1 × 75   =  75  50%           Wk3-4  25 mg SID
  Wk3  2 × 25   =  50  33%           then STOP
  Wk4  1 × 25   =  25  17%
  then STOP
```

## Relationship to the Word SOP

The parent folder (`Dropbox/.../LDH docs/`) holds `Shelter Stress Protocol v1.1.docx`, which
carries the same changes, written into the original SOP with its sentence shapes preserved and a
v1.1 row added to the version-control table. `Shelter Stress Protocol.docx` (v1.0) sits alongside
it, untouched. The tool's *What changed in SOP v1.1* table lists the differences from v1.0 for
staff who learned the old protocol.

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
