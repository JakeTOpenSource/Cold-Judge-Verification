# Records integrity

Status: procedure extracted from two records. The pilot-001 record (2026-09-23) uses the mechanisms in section 2. The custodial hash conduit v0.3 (declaration signed, frozen 2026-10-08) is a tool that implements the mechanism in section 3 and is itself kept under the mechanisms in section 2.

## 1. Purpose

To keep the record of an evaluation tamper-evident: any change to a claim, a piece of evidence, a tool, or a reported result after it enters the record is detectable, every historical finding stays reproducible against the exact files that produced it, and corrections are made in the open.

## 2. Mechanisms on record

**A. Hash manifests.** A list of every file in a set with its SHA-256, kept with the set and checked with `sha256sum -c`. This repository's `MANIFEST.sha256` is one. The hash conduit's `FREEZE.md` (23 entries) is one. The pilot-001 tool directory keeps a manifest of every version of the comparison tool.

**B. Procedure hash cited by results.** Every result file carries the hash of the procedure that produced it. A changed procedure is detected in the results, not only in the procedure directory.

**C. Lossless relay.** The party that produces a file reports its hash. The party that archives it confirms the archived hash is the same. The pilot-001 record states this for both Track C archives.

**D. Versions side by side.** When a tool is amended, the prior version is kept next to the new one with its hash, and every log entry cites the version it reproduces against. The pilot-001 record states this as a standing rule: never rebuild history in place. The rule was adopted on 2026-09-23 after an earlier verification bundle had been rebuilt in place that same day; the record says so and names the practice as superseded.

**E. Corrections as new entries.** A false statement in the record is struck, not deleted. The correction sits next to it, names the error, and states the lesson. The pilot-001 contradiction log contains one such correction, made after the independent auditor found the error.

**F. Freeze with signed declaration.** A set of files is declared frozen by a manifest of their hashes and the owner's signature on a declaration that says what the set is, what it is for, and what its limits are. Anything after the freeze is a new version. Open items are kept outside the freeze boundary until the owner rules on them.

**G. Custodial hash chain.** For an object that passes through several forms (a claim, its formal statement, a proof, a report), each form is taken into custody with its own hash commitment, and the links between adjacent forms are verified separately, so a failure is localized to the link where it happened. The hash conduit implements this; section 3 describes it.

## 3. The custodial hash conduit v0.3, as built

**What it is.** A command-line tool in one Python file, standard library only, no network, no model calls, local files only. It takes custody of four artifacts in sequence and audits the chain between them. It was built to answer one question about AI-generated formal proofs: does this proof prove what was claimed, as distinct from does this proof check. Its first target is a formal-proof pipeline, after a published case in which a machine-checked proof did not establish the claim stated in the accompanying paper.

**The four artifacts.** P1, the written claim. P2, the claim in formal, checkable form. P3, the formal proof. P4, the reported result.

**Commitment.** For each artifact the tool records a hash computed over the artifact's content together with the artifact's phase identifier and a session nonce. The nonce is minted once, at declaration, from the operating system's random source. The declaration step also writes a receipt (nonce, P1 hash, timestamp) that is meant to be committed to version control or published. The tool's session file is trusted local state; the receipt is the external anchor.

**Links.** Three links join adjacent artifacts. Each is recorded with the predecessor hash it binds to. Two of the links (P1 to P2, and P3 to P4) are translation links: whether the formal statement means the written claim, and whether the report says what the proof proved, are judgments, and the tool requires a named human reviewer to attest each one, with a note, bound to the exact hashes of both endpoints. The third link (P2 to P3) is mechanical: the formal checker must succeed on the proof, and the proof must contain a theorem ascribed to the declared claim. A checker that succeeds on a file that proves something else is a TRIP, not a pass. The tool refuses a human attestation on the mechanical link.

**Re-recording.** Recording a new version of any artifact keeps the prior version in that artifact's history. An attestation whose endpoints have changed is demoted to HOLD with the reason that it predates the current artifact; the attestation itself stays on record.

**Dependencies.** The records for P2 and P3 include hashes of locally resolvable imports and of the checker toolchain, on a best-effort basis. A changed dependency at audit is a TRIP (stale reuse); a changed toolchain is a HOLD (re-verify).

**Verdicts.** Each link reports PASS, HOLD, or TRIP as defined in `findings-classification.md`. The audit's exit status is 0 for all PASS, 2 for any HOLD, 1 for any TRIP.

**Threat model, as declared.** The tool is designed against silent drift, sloppiness, and stale reuse in high-volume pipelines, and against pipeline confusion, stale-cache reuse, and accidental re-pointing of an artifact after it was checked. It is not designed against its own operator altering the session file; the defense there is the external receipt. The checker toolchain is trusted.

**Declared limits.** The translation links need a human; the tool scopes that work and does not do it. The numeric-literal comparison between artifacts is a heuristic tripwire and is noisy on real manuscripts. Import pinning is bounded local resolution, not a build system. No real checker toolchain had run in the build environment at the freeze: the mechanical link's pass and fail paths were exercised against stand-in binaries only, and the signed declaration states this as outstanding work rather than waiving it. Timestamps are recorded but not trusted; the nonce is the only freshness mechanism.

## 4. The v0.3 freeze record

| Item | Value |
|---|---|
| Declaration | `DECLARATION.md`, status line reads: signed by Jacob Tiller, 2026-10-08. Acceptance recorded on the basis of the recorded test runs (23 of 23) and the v0.3 PDF. |
| Freeze manifest | `FREEZE.md`, 23 entries. Verify from the bundle directory with `sha256sum -c FREEZE.md`. |
| Distribution bundle | `hash-conduit-v0.3.zip`, SHA-256 `40ed8e58237e7abb0ffce8d26380a39fbd8c5b4ff73425849abeedbe1e81cc15` |
| Kill criteria | Six fixtures named in the declaration (mismatch, weakening, tamper, faithful, stale attestation, fresh session) plus the stand-in-binary branches. The declaration states that if any kill criterion failed the build was to be rejected. |
| Open item outside the freeze | A draft erratum concerning the signature date recorded in the declaration. It sits outside the freeze boundary and awaits the owner's ruling. The frozen files are unchanged. |

**Verification on 2026-10-15** by the author of this repository, on Windows 11 with Python 3.14 under Git Bash, from the distribution bundle:

- Bundle hash: matched the value above.
- Freeze manifest: 23 of 23 entries OK.
- Test suite: 19 of 23 passed. The four that did not (H, K1, K2, K3) failed in the test harness, not in the tool. One assertion compares a recorded path against a forward-slash pattern, which a Windows path does not match. Three tests use POSIX shell scripts as stand-ins for the checker binary, which Windows cannot execute (the tool records the operating system's error and reports the link as TRIP; under the taxonomy in `findings-classification.md` a checker that cannot run is a HOLD, not a TRIP; the v0.3 tool does not make that distinction and v0.4, below, does). The same binding check, rerun by hand with a Windows-executable stand-in, reported PASS with the reason that a theorem ascribed to the claim checks. The declaration's 23 of 23 is the result on the platform it was recorded on; the harness's platform assumptions are a portability limit of the test script and are noted here so that a reader on Windows is not surprised.

None of this alters the frozen state.

**v0.4, frozen 2026-10-15.** Two files changed against v0.3; every other file is byte-identical. The tool (version 0.4.0) now records a checker binary that cannot be executed as HOLD, with the reason that the checker was unavailable, instead of TRIP with the reason that the checker failed, and it records the resolved path, SHA-256, and version line of the checker that ran. The test script writes its stand-in checker binaries in a form Windows can execute, normalises path separators in one assertion, and adds two checks for the two tool changes. The declaration reproduces the v0.3 text without change, quotes the owner's written approval, and states that the signature line was recorded by the owner's agent at his instruction. Result at the freeze: 26 of 26 on Windows 11, twice, with byte-identical output, verified again from a clean extraction of the distribution zip. The POSIX branch of the test script was not executed at the freeze. A ledger tool developed alongside the conduit is not part of v0.4; its last review ended No-go.

| Item | Value |
|---|---|
| Declaration | `DECLARATION.md`, status line reads: signed by Jacob Tiller, 2026-10-15. |
| Freeze manifest | `FREEZE.md`, 23 entries. Three lines differ from v0.3: `DECLARATION.md`, `conduit.py`, `run-tests.sh`. |
| Distribution bundle | `hash-conduit-v0.4.zip`, SHA-256 `89f84517b485f653b52304403779af4ae83cc6a1db470d66d551fc7885164d94` |

## 5. Applying the chain to audit evidence

The mechanism in section 3 was built for a proof pipeline. Its intended use in an audit setting maps the four artifacts as follows: P1, the audit criteria as written; P2, the criteria in checkable form; P3, the evidence and the results of checking it; P4, the audit report. The two translation links (do the checkable criteria mean the written criteria; does the report say what the evidence showed) are attested by named reviewers. The mechanical link (does the evidence satisfy the checkable criteria) is verified by machine. A failure is localized to the link where it happened and classified under `findings-classification.md`.

This mapping is stated as the intended use. It has not yet been exercised on an audit record. The pilot-001 record used mechanisms A to E of section 2; the conduit has been exercised on its own fixtures only.

## 6. Procedure for this repository

1. Every file in the repository except `MANIFEST.sha256` is listed in `MANIFEST.sha256` with its SHA-256.
2. When any file changes, the manifest is regenerated and committed in the same commit as the change.
3. Verify with `sha256sum -c MANIFEST.sha256` from the repository root.
4. History is never rewritten. A correction is a new commit whose message says what it corrects.
5. A release is tagged only after the manifest has been verified and the owner has approved publication in words.
