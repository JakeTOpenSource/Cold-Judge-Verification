# Findings classification

Status: taxonomy extracted from two tools on record. The custodial hash conduit v0.3 (frozen 2026-10-08) reports PASS, HOLD, and TRIP directly, with exit statuses 0, 2, and 1. The pilot-001 comparison tool (2026-09-23) reports CLEAN or CONTRADICTION, and its contradiction log applies the same three classes by hand. This document makes the taxonomy formal so it can be used without either tool.

## 1. The three classes

**PASS.** Every applicable check has been verified against the audit criteria. Where a check requires a human attestation, the attestation is on record and is bound to the current bytes of what it attests. No HOLD is open and no TRIP exists.

**HOLD.** An unresolved item on the audit trail. Something required for classification is missing: evidence, an attestation, a tool, or an investigation. A HOLD is not a pass and is not a failure. It blocks acceptance until it is resolved by evidence or by the owner's recorded decision. A HOLD never resolves by the passage of time.

**TRIP.** A confirmed nonconformity. The evidence shows that something does not meet its criteria. A TRIP requires correction, cause analysis, corrective action, re-verification, and a record of all four.

There is no fourth class. In particular there is no warning class, no informational class, and no partial pass.

## 2. What triggers each class

The triggers below are the ones the two tools on record actually implement. A new tool that adopts this taxonomy should list its triggers the same way.

### TRIP triggers

From the hash conduit:

- A recomputed hash differs from the recorded hash (integrity).
- A link between adjacent artifacts binds to the wrong predecessor hash (binding).
- The session nonce does not match (freshness).
- The mechanical checker fails on the artifact it is asked to check.
- The mechanical checker succeeds but the artifact does not address the declared claim (a vacuous success).
- A pinned dependency has changed since it was recorded (stale reuse).

From the pilot-001 comparison tool, each CONTRADICTION kind:

- A value outside its declared tolerance.
- A procedure hash that does not match the public procedure files.
- A test whose status is not pass.
- A non-finite numeric value (not-a-number or infinity).
- A list of the wrong length.
- A field that the sealed expectations do not contain.
- Cross-track divergence outside tolerance.

### HOLD triggers

From the hash conduit:

- A link that requires a human attestation and has none yet.
- An attestation that predates the current bytes of the artifact it attests. The earlier attestation stays on record.
- The mechanical checker is not available, so the mechanical link cannot be verified. It is never marked as verified in its absence.
- A heuristic finding, such as a difference in the numeric literals between two artifacts. Heuristic findings are labeled as heuristic in the output and are never a TRIP.
- The formal statement does not isolate the declared claim, so the binding between statement and proof cannot be enforced mechanically.
- A custom axiom is present in the proof. It is named in the record and caps the link at HOLD; there is no axiom allow-list.
- The checker toolchain has changed since the record was made.

From pilot-001:

- Any logged contradiction that has not yet been classified.

## 3. Rules

1. **Worst class governs.** The overall finding for a record is the worst class among its items: any TRIP makes the overall finding TRIP; otherwise any HOLD makes it HOLD; otherwise PASS. Exit statuses follow: 0 for PASS, 2 for HOLD, 1 for TRIP.
2. **A heuristic finding is never a TRIP.** A tripwire that can misfire raises a HOLD for a human to examine.
3. **A human may not attest a mechanical link.** Where a check can be made by machine, only the machine's result counts. The hash conduit refuses an attestation on that link by design.
4. **A HOLD resolves by evidence or decision, not by waiting.** The resolution is recorded with what resolved it.
5. **A TRIP against the checking tool is handled like a TRIP against the object.** Correction, cause analysis, corrective action, re-verification, record.
6. **Class changes are recorded, never overwritten.** When an item is demoted, the evidence that supported the earlier class stays on record. The hash conduit keeps a superseded attestation with the reason it was superseded.
7. **Every TRIP names its trigger.** A finding that cannot name which trigger fired is a HOLD.

## 4. What each class requires of the owner

| Class | Required before acceptance | Who acts |
|---|---|---|
| PASS | Nothing further. The owner may accept. | Owner |
| HOLD | The missing evidence, attestation, tool run, or investigation, then re-classification. Or the owner's recorded decision to accept the item as a stated limit, with the reason. | Advisor or auditor supplies; owner decides |
| TRIP | Correction of the nonconformity; analysis of its cause; corrective action to prevent recurrence; re-verification showing the correction works; all four on the record. | Advisor corrects; auditor re-verifies; owner approves any amendment |

## 5. Relation to nonconformity handling in ISO/IEC 42001 Clause 10

The three classes map onto the nonconformity and corrective action process of a management system as follows. A HOLD is an item that requires further evidence before classification; it is neither a nonconformity nor a conformity yet. A TRIP is a confirmed nonconformity: the organization reacts to it (correction), evaluates the need to eliminate its cause (cause analysis), implements the action needed (corrective action), reviews the effectiveness of that action (re-verification), and retains documented information as evidence of all of it (the record). Where the corrective action changes the criteria or the tool, that is an amendment and requires the owner's approval. A PASS is a conformity.

## 6. Worked instances from the record

| Record | Item | Initial class | Resolution |
|---|---|---|---|
| pilot-001, 2026-09-23 | Two tracks differ by 5.55e-16 on one float; tolerance 1e-9 | HOLD (logged contradiction) | Classified: TRIP against the comparison tool (stricter than the clause's declared tolerance), with a false premise as secondary cause. Tool amended with owner approval, prior version kept, re-run clean, re-challenged with four probes. The object itself: no nonconformity, within tolerance by design; clause README amended to say so. |
| pilot-001, 2026-09-23 | Auditor finds a not-a-number value passes as CLEAN | TRIP against the tool | Fixed the same day in version 1.2: non-finite values raise their own contradiction kind; boolean fields must be booleans. Re-validated; both real pairs still CLEAN; nine probes return their specified verdicts. Versions 1.0 and 1.1 kept so the defect stays reproducible. |
| pilot-001, 2026-09-23 | Two statements in the contradiction log do not match the files | TRIP against the record | Both struck and corrected in place; the correction names the error and the lesson. Second audit found the correction had not been carried into one table of the evidence packet; corrected in the third bundle. |
| hash conduit v0.3, 2026-10-08 | Faithful proof with both attestations recorded; the formal checker is not installed in the build environment | HOLD on the mechanical link only | Left as HOLD in the frozen state and stated in the signed declaration as outstanding work. Not marked as verified. The owner's signature accepts the limit; it does not waive it. |
| hash conduit v0.3, 2026-10-08 | Proof file altered after the formal statement was recorded | TRIP (integrity) | Reported with exit status 1. This is a kill criterion of the declaration: a tool that failed to report it would have been rejected. |
