# LDH Shelter Stress Management Tool — Dogs

A single self-contained HTML tool supporting the LDH SOP *Shelter Stress Management – Dogs* v1.0
across the North Melbourne and Cranbourne sites.

Open `shelter-stress.html` in any browser. No server, no install, nothing external — it can be
emailed or dropped on a shared drive as-is.

## Private

This repo is **internal**. It names LDH sites, Teams channel names, Sheltermate and named staff,
so it is deliberately not published to GitHub Pages and has no public remote.

## The three modes

| Mode | For | Produces |
|---|---|---|
| **1. Start medication** | New shelter stress case | Starting dose + tablet breakdown, reassessment prompt, note for Teams/Sheltermate |
| **2. Recheck** | 48–72 h or weekly review | Maintain / increase 25% / add second drug / refer, with the new dose and pen medication instruction |
| **3. Wean and discharge** | Adoption, foster, rescue | Weaning ladder, dispensing labels with quantities, waiver text for the Sheltermate indemnity |

## Clinical rules

Trazodone 6 mg/kg BID first line for **every** dog regardless of size, +25% steps, ceiling
10 mg/kg BID. Then one second-line add-on: gabapentin 20 mg/kg BID up to 40, or pregabalin
3–5 mg/kg BID. Above either ceiling, refer to the behaviour department.

A dog on trazodone at 4 weeks should be considered for long-term behaviour medication —
consult Dr Leonie Poulter for case management.

### Stocked strengths

| Drug | Strengths | Splitting |
|---|---|---|
| Trazodone | 100 mg tablet | ¼, ½, ¾ |
| Gabapentin | 100, 300 mg capsules | none — do not open |
| Gabapentin | 600, 800 mg tablets | ¼, ½, ¾ |
| Pregabalin | 25, 75 mg capsules | none |

### Weaning

28 days or less → no weaning, stop at exit. More than 28 days → wean.

Trazodone and gabapentin step down 25% of the shelter dose per week, snapped to amounts the
stocked strengths can actually make, capped at 4 dosing weeks. Where a capsule size forces the
25% rung to round up, the ladder takes one extra week to get down to a quarter of the shelter
dose. A dog already on the smallest unit cannot have its dose reduced, so the frequency halves
instead: BID ×2 weeks, SID ×2 weeks, stop. Pregabalin always uses that frequency ladder because
its capsules cannot be split.

Every week is overridable from a dropdown; labels, quantities and the waiver text all recalculate.

Labels are grouped by **drug + strength** — one label per strength, listing every week that uses
it, so a strength spanning two weeks stays on a single bag.

## Worked examples (regression cases)

```
Gabapentin 600 mg BID            Gabapentin 400 mg BID
  Wk1  ¾ × 600 = 450   75%         Wk1  1 × 300 = 300   75%
  Wk2  ½ × 600 = 300   50%         Wk2  2 × 100 = 200   50%
  Wk3  2 × 100 = 200   33%         Wk3  1 × 100 = 100   25%
  Wk4  1 × 100 = 100   17%         then STOP
  then STOP
  Labels: 18 × 600 mg, 42 × 100 mg  Labels: 14 × 300 mg, 42 × 100 mg
```

## Changes from SOP v1.0

The tool deliberately departs from SOP v1.0 in seven places — no small-dog branch, Zylkene and
mirtazapine removed, trazodone ceiling raised to 10 mg/kg, weaning defined by duration and shape,
pregabalin frequency ladder, and the 4-week long-term medication trigger. All seven are listed in
the *Changes from SOP v1.0* table at the bottom of the tool, for discussion with the behaviour
department before the SOP is reissued.

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
