# Worked example: pilot-001

Public record: https://github.com/JakeTOpenSource/parallel-advisor-pilot-001 (MIT licence). Seven commits dated 2026-09-23 and 2026-09-24. The record is append-only. Every file path below is a path in that repository unless it says otherwise.

## 1. What pilot-001 was

The first run of the protocol in this repository, on a small deterministic test suite, before any evaluation in which judgment could drift. The clause README states the purpose: prove the loop end to end on a tiny deterministic suite before any real baseline is attempted. The repository's own README states what it is: a harness that grades the evaluation, not the models. The questions it answers are whether the answers were locked before the run, whether two independent parties reproduced them, what happened when they disagreed, and whether a stranger can re-run the whole thing.

The suite has five tests: five values from a seeded random generator; a fixed word list sorted and joined; a floating-point accumulation asserted within a declared tolerance; a negative control in which a forged input must be rejected; and an integrity tripwire that hashes the seed file.

## 2. Who held each role

| Role (as defined in `protocol/cold-judge-spec.md`) | Party in the record | Where stated |
|---|---|---|
| Owner (called the judge in the record) | Jacob Tiller | `README.md`, Roles |
| Advisor | Ed | `README.md`, Roles |
| Track M executor | Ed, through blind subagents, on Linux with Python 3.12.3 | `README.md`; `results/track-m/*.json` |
| Track C executor | Claude Code on the owner's machine, Windows with Python 3.14.6 | `README.md`; `results/track-c/*.json` |
| Auditor | Claude Code on the Track C machine, with the disclosure that it had been the Track C executor and had delivered both runs before opening the audit bundle | `results/audits/audit-pilot-001.md`, first paragraph |

## 3. The sealed set

| Item | Value |
|---|---|
| Clause and version | pilot-001, 1.0.0 |
| Procedure files | `clauses/pilot-001/run.sh`, `seeds.json`, `suite.py` |
| Procedure hash | `a5077f2f7373dc95d789e6dfd379ac41fcc3f7233e103e154bf8abd4cb34e9e6` |
| Seed registry hash | `78226a72d8a997ac7edd5f3040d3318dd6481fb386239f7df202626a902b2981` |
| Sealed expectations | `sealed/pilot-001.json`: observed values and tolerances for all five tests (1e-12 on the seeded values, 1e-9 on the float total, exact on the rest) |
| Executor rules | `clauses/pilot-001/README.md`: run exactly; do not edit seeds, code, or thresholds; a failure is a result; expected values are not needed and must not be sought |

## 4. The four runs

| Track | Run | Exit | Tests | Result file SHA-256 | Archive |
|---|---|---|---|---|---|
| M | 1 | 0 | 5 of 5 pass | `ea421cc4a39084babb82a4618c386b949079ada07255ffa345ae0e0d7ef0ba48` | `results/track-m/pilot-001-2026-09-23.json` |
| M | 2 | 0 | 5 of 5 pass | `8e95f7c19fee64c082e9e33090802e0ffa20e098e7058817d4709f35758e0c89` | `results/track-m/pilot-001-2026-09-23-run2.json` |
| C | 1 | 0 | 5 of 5 pass | `be8a1848c1d4d487803db8be07fb2cc9fe20abe7ccd901a01bbe25906685256c` | `results/track-c/pilot-001-2026-09-23.json` |
| C | 2 | 0 | 5 of 5 pass | `9d22248e6917bfb0b5ab1668584f187a173f94b7fa34020c25589c618b8c89da` | `results/track-c/pilot-001-2026-09-23-run2.json` |

Both tracks passed every sealed check on both runs. Each track reproduced its own observed values exactly from run 1 to run 2. The Track C archives are byte-identical to the executor's retained outputs (lossless relay). Source: `results/pilot-001-gold-evidence.md`.

## 5. The contradiction

The original comparison tool (version 1.0) flagged one cross-track divergence: the float total differed between tracks by 5.55e-16, against a declared tolerance of 1e-9. The log (`results/contradictions/pilot-001-2026-09-23.md`) records the reproduction command, the isolation to the platform mathematics library, the classification (detector specification as primary cause, false premise as secondary, with procedure, model, measurement, and environment-as-fault ruled out), the proposed amendment, the owner's approval on 2026-09-23, and the status after re-run. The pre-amendment tool is kept as `diff-tool/versions/diff-v1.0.py` so the contradiction stays reproducible. The full handling is described in `protocol/blind-adjudication.md`, section 5.

## 6. The independent audits

**First audit** (`results/audits/audit-pilot-001.md`, 2026-09-23). All six verification steps produced their expected outcomes and every archived hash matched. Three findings against the tool and the record: a not-a-number value in any numeric field passed as CLEAN; an integer 1 was accepted in place of a boolean; and two statements in the contradiction log did not match the files. All three were corrected the same day: the tool at version 1.2 (`diff-tool/versions/MANIFEST.md`), the log by a correction that strikes the false note and names the lesson. The auditor recorded the disclosure of non-blindness.

**Second audit** (`results/audits/audit-pilot-001-v2.md`, 2026-09-23), on the rebuilt bundle with all three tool versions side by side. Every version hash matched the manifest, the live tool was byte-identical to version 1.2, all seven verification steps passed, all nine specified tamper probes (`PROBES.md`) returned their specified verdict and kind, both historical defects reproduced against the versions named, and five auditor-added probes were all caught. The only defects were documentary: one table cell and one status line not yet carried forward from the correction. A third bundle fixed those with every executable file byte-identical to the second (`VERIFY.md`, first paragraph).

## 7. The acceptance decision

Accepted by the owner on 2026-09-23 (commit `a52b80e`, "Mint pilot-001 as gold"). The accepted state, from `results/pilot-001-gold-evidence.md`, Mint section: clause pilot-001 version 1.0.0 with the procedure hash above; the four result hashes above; comparison tool version 1.2, hash `6d52f9653439a521e55b45286c83b711aaf36de7646c66ef16b68514d11aa1a6`, CLEAN on both run pairs; one contradiction, classified and resolved by approved amendment; two independent audit reports with the claim holding on every executable check.

## 8. What it proves and what it does not

In the record's own scope (`README.md`, "What it does not prove"; `VERIFY.md`, "Honest limits"): the pilot proves the evaluation machinery. It does not prove that the executors are capable, honest in general, or aligned; the suite was deterministic and they ran code. It does not prove the protocol works on judgment-based work where an executor could drift. Executor blindness is attested by the executors' reports, not cryptographically proven. Recomputable means bit-exact on the same platform family and within the declared tolerances across platforms, not bit-identical everywhere.

## 9. Re-verification on 2026-10-15

Performed by the author of this repository, from a fresh clone of the public repository at commit `2b79b0e`, on Windows 11 with Python 3.14.6 under Git Bash. The clone was made with `git -c core.autocrlf=false` after a first clone with the default setting had converted line endings and, correctly, produced a procedure-tamper contradiction from the tool.

| Check | Result |
|---|---|
| Procedure hash recomputed from the three procedure files (`VERIFY.md` step 2) | `a5077f2f...` as recorded |
| Public procedure run on this machine (`VERIFY.md` step 1) | Exit 0. Four of five sealed fields bit-exact; float total within 5.55e-16 of the sealed value against tolerance 1e-9 |
| Live tool (version 1.2) on the run-2 pair (`VERIFY.md` step 3) | CLEAN, exit 0 |
| Tool version 1.0 on the run-1 pair (`VERIFY.md` step 5) | Exactly one contradiction, cross-track divergence, exit 1 |
| Tool version hashes against `diff-tool/versions/MANIFEST.md` | All three match; live tool equals version 1.2 |
| Four result archive hashes against section 4 | All four match |
| Probe 2, forged procedure hash | Procedure tamper, exit 1, as specified |
| Probe 4, not-a-number total, against version 1.2 | Non-finite value, exit 1, as specified |
| Probe 4 against version 1.1 | CLEAN, exit 0: the historical defect reproduces as the record says |
| Probe 9, change inside tolerance | CLEAN, exit 0, as specified |

The remaining probes were not run on 2026-10-15; the second audit reports all nine. Disclosure: in performing this re-verification the author of this repository read the sealed values and is not a blind executor for pilot-001.

## 10. Where each requirement of the specification is evidenced

| Requirement | Evidence in the public record |
|---|---|
| R1 Independence of the evaluator | `README.md`, Roles; auditor's disclosure in `results/audits/audit-pilot-001.md` |
| R2 Criteria before evidence | `sealed/pilot-001.json`; `VERIFY.md` steps 1 and 2 |
| R3 Blind execution | `clauses/pilot-001/README.md`, Rules for executors; `VERIFY.md`, Honest limits |
| R4 Two tracks, different platforms | `results/track-m/`, `results/track-c/`, environment fields |
| R5 Every contradiction stops the process | `results/contradictions/pilot-001-2026-09-23.md` |
| R6 Classification by cause | same file, Classify section |
| R7 Amendments approved and re-run | same file, Proposed amendment and Status; `diff-tool/versions/MANIFEST.md` |
| R8 Independent audit of checker and record | `results/audits/` (two reports); `PROBES.md` |
| R9 Disclosure of non-blindness | `results/audits/audit-pilot-001.md`, first paragraph; `results/pilot-001-gold-evidence.md`, finding 3 |
| R10 Tamper-evident records | procedure hash in every result file; `diff-tool/versions/MANIFEST.md`, standing rule |
| R11 Declaration first, gates throughout | owner approval of the amendment on 2026-09-23; acceptance recorded with the exact state, Mint section |
| R12 Scope stated with the result | `README.md`, What it does not prove; `VERIFY.md`, Honest limits |
