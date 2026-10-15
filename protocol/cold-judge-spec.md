# Cold-judge specification

Status: extracted from applied work. First stated as a standalone rule on 2026-09-25. The pilot-001 evaluation of 2026-09-23 is the first recorded application; the custodial hash conduit v0.3 of 2026-10-08 is the second.

## 1. Purpose

This document states the cold-judge principle in plain language, defines the terms used by every other document in this repository, and lists the requirements an evaluation must meet to be called cold-judged. It is written for a reader who audits management systems and has not seen any of the author's other work.

## 2. The principle

An evaluator is trustworthy only when two conditions hold at once:

1. The evaluator has no stake in the outcome.
2. The evaluator did not share the working context of the party whose work is being evaluated.

When the second condition fails, the evaluator is said to be in context. An evaluator in context fails in two predictable ways. First, it drifts toward approval: the producer's framing of the problem becomes the evaluator's framing, and the evaluator confirms what the producer expected to find. Second, it drifts toward the letter of the measure: it satisfies the stated check rather than the objective the check was meant to serve. In the field this work comes from, these two failures are called sycophancy and reward hacking. In auditing terms they are a loss of impartiality and a loss of validity of the audit criteria.

An evaluator that meets both conditions is called a cold judge. The word cold means only that the evaluator arrives without the producer's context. It does not mean hostile, and it does not mean uninformed about the criteria.

A cold judge needs an independent signal to judge against. That signal cannot come from the producer and cannot be constructed after the evidence exists. The three mechanisms in this repository exist to supply it:

- Sealed expectations supply criteria that were fixed before any evidence was collected.
- Blind adjudication supplies evidence gathered by parties who could not see the criteria, on two independent tracks, so that agreement between the tracks is informative and disagreement is detectable.
- Independent audit supplies a check on the checker: the auditor re-runs every step, attacks the checking tool with specified mutations, and is briefed to look for errors in the advisor's own record.

## 3. Defined terms

Terms are defined once here and used with these meanings throughout the repository.

**Object under evaluation.** The thing being judged. In pilot-001 it was a frozen test procedure and the outputs of running it. In the hash conduit it is a chain of four artifacts: a written claim, its formal statement, a formal proof, and a reported result.

**Audit criteria.** The reference the object is judged against. In this protocol the audit criteria are the sealed expectations plus the declared tolerances.

**Owner.** The person who sets the audit criteria, approves every amendment to the criteria or to a checking tool, resolves contradictions, and issues the acceptance decision. The owner's approval is given in words and is recorded. The pilot-001 record uses the word judge for this role.

**Advisor.** The party who writes the procedure, computes and seals the expectations, compares outputs against the expectations and against each other, investigates and classifies contradictions, and maintains the record. The advisor does not execute blind and does not issue the acceptance decision.

**Executor.** A party who runs the frozen procedure exactly as written, has no access to the expectations and does not seek them, and delivers the output unchanged. An executor who alters, corrects, or interprets the output has left the role.

**Track.** One executor on one platform. The protocol requires two tracks on different platforms. The pilot-001 record names them Track M and Track C.

**Auditor.** An independent party who re-runs every check in the record, attacks the checking tool with specified mutations and with mutations of the auditor's own choosing, and examines the advisor's record for false statements. The auditor is briefed to check the advisor's record, not only the executors' outputs.

**Disclosure of non-blindness.** The recorded statement that a party has seen the sealed expectations and may not serve as a blind executor for that evaluation again.

**Sealed expectations.** The expected outputs of the procedure, computed by the advisor from the public procedure before any executor runs, recorded with a hash, and recomputable by any reader from the public procedure. See `sealed-expectations.md`.

**Procedure hash.** A single hash computed over every file that defines the procedure, so that a change to any of them is detectable in every result file that cites the hash.

**Tolerance.** A declared numeric bound on acceptable disagreement for a specific field. Tolerances are part of the audit criteria and are set before execution.

**Contradiction.** Any disagreement between a result and the sealed expectations, or between the two tracks, outside the declared tolerances. A contradiction stops the process and enters the contradiction protocol in `blind-adjudication.md`. No contradiction is averaged, rounded, or waived.

**Classification by cause.** The step in which a contradiction is attributed to one or more named causes, with the ruled-out causes also named. The recorded classes are defined in `blind-adjudication.md`.

**Amendment.** A change to the audit criteria, the procedure, or a checking tool. An amendment requires the owner's approval in words and a mandatory re-run.

**HOLD.** An unresolved item on the audit trail that requires further evidence before it can be classified. See `findings-classification.md`.

**TRIP.** A confirmed nonconformity requiring correction, cause analysis, and corrective action on the record. See `findings-classification.md`.

**PASS.** Every applicable check verified against the audit criteria, with no open HOLD and no TRIP.

**Declaration.** The owner's written statement, given before the work, of what is to be built, run, or published, and against what criteria. Declaration-first means the declaration precedes the work. A proposal from any other party is not a declaration.

**Gate.** A point at which work stops until the owner says in words to continue. Gates are conversational, not timed.

**Record.** Every artifact the evaluation produced or relied on: procedure files, sealed expectations, result files, comparison outputs, contradiction logs, audit reports, and the hashes of each. Records are append-only. A correction is a new entry that names what it corrects. Superseded entries are marked, not removed.

**Freeze.** A declared state of a set of files, fixed by a manifest of their hashes and by the owner's signature. Anything after the freeze is a new version, never an edit to the frozen state.

## 4. Requirements

An evaluation conforms to this specification when all of the following hold. Each requirement names the procedure document that carries it out.

**R1. Independence of the evaluator.** No party who produced the object under evaluation, or who shared its working context, acts as owner, auditor, or blind executor for it. The record states who held each role. (`blind-adjudication.md`)

**R2. Criteria before evidence.** The audit criteria are computed, recorded, and hashed before any track executes. The criteria are recomputable from the public procedure, so that any later tailoring of the criteria would be detectable by recomputation. (`sealed-expectations.md`)

**R3. Blind execution.** No executor has access to the criteria during execution. Executors deliver output unchanged. Blindness is attested in the record; the record also states that attestation is not cryptographic proof. (`blind-adjudication.md`)

**R4. Two tracks, different platforms.** Two executors run the identical frozen procedure on different platforms. Acceptance requires both tracks to pass. (`blind-adjudication.md`)

**R5. Every contradiction stops the process.** A disagreement of any size outside the declared tolerance, between a track and the criteria or between the two tracks, halts the evaluation and is entered in the contradiction log before any other step is taken. (`blind-adjudication.md`)

**R6. Classification by cause on the record.** Each contradiction is isolated to a mechanism, attributed to named causes, and the ruled-out causes are also named. (`blind-adjudication.md`)

**R7. Amendments are approved and re-run.** No change to criteria, procedure, or checking tool takes effect without the owner's approval in words. Every amendment is followed by a re-run of the affected checks, and the pre-amendment tool is kept so the original finding stays reproducible. (`blind-adjudication.md`, `records-integrity.md`)

**R8. Independent audit of the checker and the record.** An auditor who did not write the checking tool re-runs every step, applies the specified tamper probes and probes of the auditor's own choosing, and reads the advisor's record for false statements. Findings against the checker and against the record are logged and corrected with the same formality as findings against the object. (`blind-adjudication.md`)

**R9. Disclosure of non-blindness.** Any party who has seen the criteria is recorded as non-blind and is not used as a blind executor for that evaluation again. (`blind-adjudication.md`)

**R10. Tamper-evident records.** Every artifact in the record is hashed. Result files cite the procedure hash. Checking-tool versions are kept side by side with their hashes. History is never rewritten in place. (`records-integrity.md`)

**R11. Declaration first, gates throughout.** Nothing is built, run, logged, or published without the owner's declaration. Work stops at each gate until the owner says to continue. The acceptance decision is the owner's alone and is recorded with the exact state accepted. (`sealed-expectations.md`, `records-integrity.md`)

**R12. Scope stated with the result.** Every result states what it proves and what it does not. An evaluation of a deterministic procedure proves the evaluation machinery, not the judgment of the executors, and the record says so. (`evidence/pilot-001-summary.md`)

## 5. Findings

Findings use three classes and no others: PASS, HOLD, TRIP. The classes, what triggers each, and what each requires are defined in `findings-classification.md`. A tool that reports findings also reports an exit status that distinguishes the three, so the classes can be acted on by other tools without interpretation.

## 6. Limits of the specification

- The specification governs the evaluation. It does not govern the object under evaluation and does not make the object correct.
- The specification has been applied, as of the date at the top of this document, to a deterministic test procedure and to a four-artifact proof chain. It has not been applied to an evaluation in which an executor's judgment could drift. The record states this as the next test, not as a result.
- Blindness is attested, not proven. For a deterministic procedure the executor cannot tailor the output in any case. For a judgment-based procedure, attestation alone would not be enough, and the specification does not yet say what would be.
- The specification does not defend against an owner or advisor who falsifies the record. The defenses against that are external anchoring of the record's hashes, described in `records-integrity.md`, and independent audit under R8.

## 7. Lexicon

The protocol terms above are the author's. Each corresponds to an established term in management-system auditing, listed here so that a reader who works from those standards knows which concept is meant. This is a vocabulary table, not a conformity claim: nothing in this repository claims that any organization or process meets any standard.

| Protocol term | Established term | Where the established term is defined |
|---|---|---|
| Cold-judge principle (R1, R9) | Impartiality: the evaluator has no stake in the outcome and no prior involvement with the object under evaluation | ISO/IEC 17021-1, Clause 5.2; ISO/IEC 42006, impartiality requirements |
| Sealed expectations (R2) | Audit criteria documented prior to evidence collection | ISO/IEC 17021-1, audit planning |
| Blind adjudication, two-track execution (R3 to R6) | Audit evidence corroboration: findings traceable to independently gathered evidence; contradictions classified and resolved on the record | ISO/IEC 17021-1 |
| HOLD and TRIP (R5, R7, R8) | Audit findings classification and nonconformity handling. HOLD: an unresolved audit-trail item requiring further evidence before classification. TRIP: a confirmed nonconformity requiring correction, root-cause analysis, and corrective action | ISO/IEC 42001, Clause 10 |
| Declaration first, gated phases, owner approves the exact output (R11) | Audit programme and audit plan; a readiness review before the implementation audit; findings reviewed and confirmed with the auditee before conclusions are issued | ISO/IEC 17021-1 |
| Records integrity, hash conduit (R10) | Control of documented information; audit records integrity: tamper-evident integrity of claims, evidence, and reported results | ISO/IEC 42001, Clause 7.5; ISO/IEC 17021-1 |
