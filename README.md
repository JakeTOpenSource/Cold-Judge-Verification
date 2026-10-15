# Cold-Judge-Verification

A published record of one evaluation methodology and the evidence that it was applied. The methodology is written here in the language of management-system auditing: audit criteria, audit evidence, findings, nonconformities, corrective action, and control of records. That vocabulary is used because it is precise and widely understood; this repository is not about any standard.

The central rule is this: an evaluator is trustworthy only when it has no stake in the outcome and did not share the working context of the party whose work it evaluates. The protocol in this repository enforces that rule with three mechanisms: expectations sealed before any evidence exists, execution by parties who cannot see the expectations, and an independent audit that is briefed to check the record of the party who ran the evaluation.

Read this file first, then `protocol/cold-judge-spec.md`. The remaining documents are the four procedures and a worked example, in that order.

## What this repository is

- A plain-language specification of the cold-judge principle and its defined terms.
- Four procedures extracted from work that was actually carried out between 2026-09-23 and 2026-10-08: sealed expectations, blind two-track adjudication, a two-class findings taxonomy (HOLD and TRIP), and tamper-evident control of records.
- One worked example: the pilot-001 evaluation, whose complete receipts are public at https://github.com/JakeTOpenSource/parallel-advisor-pilot-001.
- A SHA-256 manifest of every file in this repository.
- A licence (section below) that keeps the documents attributed and open for non-commercial sharing and adaptation.

Everything here describes work that is finished and on record. Dates, hashes, and counts in these documents are taken from the records they cite, and the citing document says where each one comes from.

## What this repository is not

- It is not a certificate, an accreditation, or a credential. The author holds none of these in relation to the standards cited and does not claim any. No certification body is represented here, and nothing in this repository authorizes anyone to issue a certificate.
- It is not an audit of any product, vendor, or organization. No system other than the author's own evaluation harnesses was examined.
- It is not legal advice, and it does not describe a service, a plan, or an offering of any kind.
- It is not a claim that the protocol proves an AI system safe, correct, or aligned. The worked example proves the evaluation machinery on a deterministic test suite. The repository says so wherever the question arises.
- It is not a mapping to any standard. Where a management-system standard is named (ISO/IEC 17021-1, ISO/IEC 42006, ISO/IEC 42001), it is named once, in the specification's lexicon, to say which established term a protocol term corresponds to. Clause numbers and short titles only; no standard's text is reproduced.

## Contents

```
README.md                          this file
protocol/
  cold-judge-spec.md               the principle, defined terms, numbered requirements
  sealed-expectations.md           procedure: fixing audit criteria before evidence collection
  blind-adjudication.md            procedure: two independent tracks, contradiction handling
  findings-classification.md       taxonomy: PASS, HOLD, TRIP, and what each requires
  records-integrity.md             procedure: tamper-evident custody of claims, evidence, results
evidence/
  pilot-001-summary.md             worked example, with links into the public record
LICENSE                            CC BY-NC-SA 4.0 legal code, verbatim
MANIFEST.sha256                    SHA-256 of every file above
```

## How to verify this repository

From the repository root, with GNU coreutils available:

```
sha256sum -c MANIFEST.sha256
```

Every line should report `OK`. The manifest lists every file in the repository except itself. It is regenerated whenever a file changes, and the change and the new manifest are committed together. History is never rewritten; corrections are made as new commits that say what they correct.

The worked example can be verified independently of this repository. The public pilot-001 repository ships its own verification instructions (`VERIFY.md`, seven steps, Python 3 standard library only) and nine specified tamper probes (`PROBES.md`). `evidence/pilot-001-summary.md` reports the result of running those steps on 2026-10-15.

## Terms used in this repository

Full definitions are in `protocol/cold-judge-spec.md`. The short forms:

- **Owner.** The person who sets the acceptance criteria, approves every change to criteria or checking tools, and issues the acceptance decision. In every record cited here the owner is Jacob Tiller. The pilot-001 record calls this role the judge.
- **Advisor.** The party who writes the procedure, seals the expectations, compares the outputs, and investigates disagreements. The advisor never executes the procedure blind and never issues the acceptance decision.
- **Executor.** A party who runs the frozen procedure exactly as written, without access to the expectations, and reports the output unchanged. Two executors on different platforms form the two tracks.
- **Auditor.** An independent party who re-runs every check, attacks the checking tool with specified mutations, and examines the advisor's own record for errors. Once an auditor has seen the sealed expectations, that party may not serve as a blind executor for the same evaluation again.
- **Sealed expectations.** The audit criteria for one evaluation, fixed and hashed before any evidence is collected, and recomputable by anyone from the public procedure.
- **Contradiction.** Any disagreement between a result and the sealed expectations, or between the two tracks. A contradiction stops the process. It is never averaged away.
- **HOLD.** An unresolved item on the audit trail. Further evidence is required before it can be classified. A HOLD is not a pass and is not a failure.
- **TRIP.** A confirmed nonconformity. It requires correction, cause analysis, and corrective action, all on the record.
- **PASS.** Every applicable check verified against the sealed criteria, with no open HOLD and no TRIP.
- **Declaration-first.** Nothing is built, run, logged, or published until the owner has asked for it in so many words. A proposal is not an approval.

## Provenance

| Record | Date | Where |
|---|---|---|
| pilot-001 evaluation: sealed, executed on two tracks, contradiction resolved, independently audited twice, accepted by the owner | 2026-09-23 | https://github.com/JakeTOpenSource/parallel-advisor-pilot-001 |
| Cold-judge principle first written down as a standalone rule | 2026-09-25 | owner's working notes; stated in `protocol/cold-judge-spec.md` |
| Custodial hash conduit v0.3: declaration signed, freeze manifest recorded (23 entries), test suite recorded at 23 of 23 | 2026-10-08 | distribution bundle `hash-conduit-v0.3.zip`, SHA-256 `40ed8e58237e7abb0ffce8d26380a39fbd8c5b4ff73425849abeedbe1e81cc15`; described in `protocol/records-integrity.md` |
| This repository assembled from the records above | 2026-10-15 | here |

The author of the underlying work is Jacob Tiller. The pilot-001 record names the other parties and their roles; this repository repeats those attributions and adds none.

## Licence

Cold-Judge-Verification, copyright 2026 Jacob Tiller, is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International licence (CC BY-NC-SA 4.0). The full legal code is in `LICENSE`, reproduced verbatim from https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode.txt. In short: you may copy, share, and adapt these documents for non-commercial purposes, provided you credit the author, indicate any changes, and distribute adaptations under the same licence. Other terms are available from the author on request.

The licence covers the text of this repository. It does not cover the public pilot-001 repository, which carries its own MIT licence, and it does not restrict anyone from applying the methods described here.
