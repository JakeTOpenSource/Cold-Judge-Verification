# Blind adjudication

Status: two-track procedure with contradiction handling, extracted from the pilot-001 record of 2026-09-23. The public record is at https://github.com/JakeTOpenSource/parallel-advisor-pilot-001: the executor rules in `clauses/pilot-001/README.md`, the comparison tool in `diff-tool/`, the contradiction log in `results/contradictions/`, and the two audit reports in `results/audits/`.

## 1. Purpose

To gather evidence by parties who cannot see the audit criteria, on two independent tracks, so that agreement between the tracks is informative and disagreement is detectable; and to handle every disagreement on the record, by cause, before any conclusion is drawn.

## 2. Separation of roles

The roles are defined in `cold-judge-spec.md`. The separation rules are:

- The advisor who sealed the expectations does not execute a track.
- Executors do not communicate with each other and do not see each other's outputs until both are archived.
- The auditor did not write the comparison tool and had no part in sealing.
- The owner issues the acceptance decision and no one else does.
- The record names who held each role. In pilot-001: advisor and Track M, Ed; Track C, Claude Code on the owner's machine; owner, Jacob Tiller; auditor, Claude Code on the Track C machine, with the disclosure recorded that this party was the Track C executor and had delivered both runs before opening the audit bundle.

## 3. Track execution

1. Each executor receives the frozen procedure and the executor rules. The rules state: run the procedure exactly; do not edit seeds, code, or thresholds; a failure is a result and is reported unchanged; expected values are not needed and must not be sought; the environment requirements (in pilot-001: POSIX shell, Python 3 standard library, no network).
2. Each executor runs from a cold shell on its own platform. The result file records the platform and interpreter version, the procedure hash, the seed registry hash, each test's status and observed values, and timestamps marked informational.
3. The executor delivers the result file unchanged and reports its hash. The advisor archives the file and confirms that the archived hash equals the hash the executor reported. This confirms lossless relay.
4. Each track runs twice. On the same machine the observed values must reproduce exactly; the two files may differ only in informational timestamps.

## 4. Comparison

The comparison tool performs two checks and reports one verdict.

1. **Sealed check.** Each result file is compared against the sealed expectations field by field, applying each field's declared comparison rule. The result's procedure hash and seed registry hash must equal the sealed values. Every test status must be a pass.
2. **Cross-track check.** The two tracks' observed values are compared field by field, applying the same declared tolerances.
3. **Verdict.** CLEAN when both checks pass, otherwise CONTRADICTION, with one entry per disagreement giving the field, the kind of disagreement, and the values. The exit status is 0 for CLEAN and 1 for CONTRADICTION so other tools can act on it. The kinds recorded in the pilot-001 tool are: value outside tolerance, procedure hash mismatch, test status not pass, non-finite numeric value, list length mismatch, unexpected field, and cross-track divergence.

Under the findings taxonomy in `findings-classification.md`, CLEAN is a PASS, and each CONTRADICTION entry is a HOLD until it has been classified.

## 5. Contradiction protocol

Every contradiction, of any size, goes through these steps in order. None may be skipped and none may be resolved by averaging, rounding, or discarding a value.

1. **Stop.** No further comparison, amendment, or reporting until the entry is logged.
2. **Log.** Record the exact command that reproduces the contradiction, the values on each side, and the difference.
3. **Isolate.** Trace the disagreement to a mechanism. State what was examined and what was found.
4. **Classify.** Attribute the contradiction to one or more named causes, marking one primary, and name the causes that were considered and ruled out, with the evidence for ruling each out. The classes used in the record are:
   - **Detector specification.** The comparison tool is stricter or looser than the criteria it serves. (Recorded as DETECTOR-SPEC.)
   - **Premise.** A tacit assumption in the design is false. (Recorded as PREMISE.)
   - **Procedure.** The procedure differed between tracks or from the frozen procedure. (Recorded as PROCEDURE.)
   - **Model.** An error of judgment by an executing party. (Recorded as MODEL.)
   - **Measurement.** The measured quantity is itself at fault. (Recorded as SENSOR.)
   - **Environment.** The platform or environment differs in a way that affects the result. (Recorded as RIG.)
5. **Propose.** If the classification calls for a change to the criteria, the procedure, or the tool, write the proposed amendment with its reason. A proposal is not an amendment.
6. **Approve.** The owner approves or declines in words. The approval is recorded with its date.
7. **Apply and keep.** Apply the amendment as a new version. Keep the previous version of the tool with its hash so the original contradiction stays reproducible against it.
8. **Re-run and re-challenge.** Re-run the affected checks. Then attack the amended tool with tamper probes to confirm it still catches what it caught before. Record the results.
9. **Close.** Update the status line of the log. A closed contradiction is either a TRIP that has been corrected on the record or an item closed as no nonconformity with the reason stated.

### The pilot-001 instance

The two tracks disagreed on one float by 5.55e-16, inside the clause's declared tolerance of 1e-9. The original comparison tool demanded exact equality across tracks and flagged it. The mechanism was isolated to the platform mathematics library (Linux against Windows) over a thousand summed terms. Classification: primary, detector specification (the tool was stricter than the clause's own tolerance); secondary, premise (bit-identical floats across platforms was never a safe assumption). Ruled out: procedure (hashes matched on both tracks), model (no judgment was involved), measurement (both readings valid under the clause), environment as a fault (the difference was real and within the declared band). The amendment, approved by the owner on 2026-09-23, made the cross-track check honor declared tolerances. Tool version 1.0 was kept alongside version 1.1. The amended tool was re-run (CLEAN) and re-challenged with four probes (all caught). The clause README was amended to state the platform note.

## 6. Independent audit

The auditor receives a bundle containing everything a stranger needs: the verification instructions, the sealed file, the comparison tool and every prior version of it with hashes, the procedure files, every result archive, the contradiction log, and the evidence packet. The audit brief is:

1. Record a hash manifest of the bundle at extraction and re-check it after every step.
2. Run every verification step as written, in a separate working copy. Record the exact command and output for each.
3. Apply every specified tamper probe and record whether each produced its specified verdict and kind.
4. Add probes of the auditor's own choosing.
5. Read the advisor's record (the log and the evidence packet) for statements that do not match the files. Check every file claim against its hash.
6. State the verdict: whether the claim holds as scoped by the verification instructions, and the single weakest point found.
7. Record the disclosure of non-blindness.

Findings against the tool and against the record are handled with the same formality as findings against the object. In pilot-001 the first audit found that the tool passed a non-finite number as CLEAN and accepted an integer in place of a boolean; both were corrected in version 1.2 the same day, re-validated, and the earlier versions were kept so the defect stays reproducible. The same audit found two statements in the contradiction log that did not match the files; both were struck and corrected in place, with the correction naming the error and the lesson. A second audit of the rebuilt bundle found only documentary defects, which were corrected in a third bundle with every executable file byte-identical to the second.

## 7. Acceptance

The acceptance decision is the owner's alone. The record of the decision states the exact accepted state: the clause version and procedure hash, the hash of each result archive, the version and hash of the comparison tool, the status of every contradiction, and the paths of the audit reports. Acceptance requires two tracks passing on both runs, sealed checks passing, the cross-track check clean, every contradiction closed on the record, and the independent audit's verdict that the claim holds.

## 8. Limits

- Blindness is attested in the executors' reports, not proven. For a deterministic procedure the executor cannot tailor the output anyway.
- The pilot-001 procedure was deterministic. The procedure above has not yet been run on work in which an executor's judgment could drift. The record states this as the next test.
