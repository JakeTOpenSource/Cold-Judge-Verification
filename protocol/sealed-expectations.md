# Sealed expectations

Status: procedure extracted from the pilot-001 record of 2026-09-23. The templates it was extracted from are public: the clause directory `clauses/pilot-001/`, the sealed file `sealed/pilot-001.json`, and steps 1 and 2 of `VERIFY.md`, all at https://github.com/JakeTOpenSource/parallel-advisor-pilot-001.

## 1. Purpose

To fix the audit criteria before any evidence is collected, in a form any reader can recompute from the public procedure, so that no party can tailor the criteria after seeing results and no reader has to trust the advisor's word that the criteria came first.

## 2. Inputs

- The procedure: every file an executor needs to run it, and nothing an executor must not see. In pilot-001 this was a runner script, a seed file, the test suite, and a README that is the entire public specification.
- A comparison rule for every expected field: exact match, or a numeric tolerance with its value.
- The owner's declaration that this is the procedure to seal.

## 3. Procedure

1. **Freeze the procedure.** Compute one hash over every procedure file, taking each file's name and content in a fixed order. Record it as the procedure hash. Every result file produced later must cite this hash, so that a changed procedure is detectable in the results themselves.
2. **Declare the comparison rules.** For each expected field, record whether an exact match is required or a tolerance applies, and the tolerance value. Tolerances live in the sealed file next to the values they govern, not in a separate document.
3. **Compute the expectations.** The advisor runs the public procedure on the advisor's own platform and records the observed value of every field. Nothing in the sealed file may come from anywhere but this run of the public procedure.
4. **Seal.** Record the sealed file's hash and the time. State in the record that the file's timestamp is weak evidence (timestamps can be forged) and that recomputability is the evidence that carries weight: if the sealed values had been altered after the fact, they would no longer equal what the public procedure computes.
5. **Withhold.** Executors receive the procedure and the executor rules only. The sealed file is not sent to any executor, and the executor rules say in words that expected values are not needed and must not be sought.
6. **Publish the procedure.** A stranger must be able to recompute the expectations from the published files alone, with no tool that is not freely available.
7. **Gate.** Execution does not begin until the owner has approved the sealed state in words.

## 4. Acceptance criteria for a sealed set

A sealed set is complete when all of the following are true:

- Every field in the sealed file is derivable from the public procedure by a reader with no other information.
- Every field has a declared comparison rule.
- The procedure hash is recorded and is the hash that result files will cite.
- The executor rules forbid seeking the expectations.
- The owner's approval of the sealed state is recorded.

## 5. Platform note, from the record

Numeric results that depend on the platform's mathematics library are not bit-identical across operating systems. In pilot-001 the sealed float total was computed on Linux. The Windows track reproduced it to within 5.55e-16 against a declared tolerance of 1e-9. The clause's public README was amended on 2026-09-23, with the owner's approval, to state what recomputable means: bit-exact on the same platform family, within the declared tolerance across platforms, and exact repeat on the same machine from run to run. A sealed set that depends on such values must declare a tolerance for them or it will contradict itself on the second platform.

## 6. What the procedure does not do

- It does not prove that an executor was blind. Blindness is attested by the executor's report. For a deterministic procedure the executor could not tailor the output in any case.
- It does not prevent the advisor from changing both the procedure and the sealed file before execution. That is simply a new sealed set. After execution, any change to the procedure is detected because result files cite the earlier procedure hash.
- It does not make the procedure a good test of anything. It only guarantees that the criteria came first and can be checked.

## 7. The pilot-001 sealed set

| Item | Value in the record |
|---|---|
| Clause and version | pilot-001, 1.0.0 |
| Procedure files | `run.sh`, `seeds.json`, `suite.py` |
| Procedure hash | `a5077f2f7373dc95d789e6dfd379ac41fcc3f7233e103e154bf8abd4cb34e9e6` |
| Seed registry hash | `78226a72d8a997ac7edd5f3040d3318dd6481fb386239f7df202626a902b2981` |
| Fields and rules | five seeded random values, tolerance 1e-12; one sorted string, exact; one float total, tolerance 1e-9; one boolean rejection flag, exact; one seed-file hash, exact |
| Sealed file | `sealed/pilot-001.json` |

On 2026-10-15 the author of this repository recomputed the procedure hash from a fresh clone of the public repository, with line-ending conversion disabled, and obtained the value above. Running the public procedure on Windows with Python 3.14.6 reproduced four of the five sealed fields bit-exact and the float total to within 5.55e-16 of the sealed value, inside the declared tolerance of 1e-9, which is the same platform difference the record describes. The same check, when run on a clone that had converted line endings to the Windows convention, produced a different hash and a procedure-tamper contradiction from the comparison tool. That is the mechanism working as designed: the procedure hash detects a byte-level change in the procedure files whatever its cause. A reader verifying on Windows should clone with `git -c core.autocrlf=false clone`.
