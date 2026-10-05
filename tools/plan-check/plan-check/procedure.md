# Procedure: how this skill grades a plan package

## Read order

1. Read the **Repo facts** block first. Note the contribution policy (AI disclosure requirement, PR scope guidance, review bandwidth). These facts constrain the comment-fits-thread check.
2. Read the **Issue** section. Note the reported symptom and expected behavior.
3. Read the **Thread highlights**. Note any maintainer guidance or active discussion.
4. Read the **Repro evidence** section carefully. Write down: (a) the specific observed behavior the steps pin down, and (b) the component or call site the evidence points at. This anchors diagnosis-grounded and symptom-vs-cause.
5. Read the **Candidate plan**. Note the stated cause, the scope boundary (In/Out), the files named, the approach steps, and the test plan.
6. Read the **Candidate plan comment**. Note whether it matches the plan, respects the repo policy, and avoids deadline promises.

The order matters: reading the repro evidence before the plan prevents anchoring on the plan's framing when checking whether the diagnosis is grounded.

## Evidence gathering

- **diagnosis-grounded**: From the repro evidence, extract the exact observed behavior (error message, wrong output, failing step). From the plan, extract the stated cause. Pair them.
- **scope-bounded**: From the plan, extract the In/Out scope statement. If no explicit boundary exists, treat the set of files named in Approach as the implicit scope.
- **symptom-vs-cause**: From the repro evidence, identify the code location the evidence points at (the function, module, or component the steps exercise). From the plan's Approach, identify where the change is made.
- **executable-by-stranger**: From the plan's Approach and Files sections, extract the named files and change description. Ask: could someone open those files and know what to change?
- **test-plan-observable**: From the plan's Test plan section, extract the command or test named and the stated outcome.
- **comment-fits-thread**: From the plan comment, extract any deadline or certainty claims. Compare against repo facts (contribution policy) and thread highlights (maintainer guidance).

## Check execution

Run checks in this order (each check uses only already-gathered evidence):

1. **diagnosis-grounded** — Compare the stated cause to the repro evidence's observed behavior. Pass if the cause is something the evidence directly shows. Fail if the cause contradicts the evidence or introduces a component not mentioned in the evidence. Grade unclear if the plan names a cause but the repro evidence is ambiguous about whether it applies.
2. **scope-bounded** — Check whether the plan names specific files or components In scope and excludes unrelated behavior Out. Pass if both In and Out are stated. Fail if the scope is unbounded ("fix the whole thing") or absent. Grade unclear if the scope exists but is too vague to judge boundedness (e.g., "the relevant module").
3. **symptom-vs-cause** — Compare the approach's target (files, functions) to the code location the evidence implies. Pass if the approach targets the cause location. Fail if the approach only patches the symptom (output or UI layer) while the evidence points at a deeper cause. Grade unclear if the evidence does not point clearly at any code location.
4. **executable-by-stranger** — Check whether named files and change description are specific enough for a stranger to start. Pass if at least one file is named and the change is described in terms a contributor could execute. Fail if the approach is only vague direction (e.g., "find the right place", "poke around"). Grade unclear if files are named but the change description is missing.
5. **test-plan-observable** — Check the test plan for a runnable command or named test and a concrete observable outcome. Pass if both are present. Fail if the test plan is absent or describes only a subjective outcome ("it works"). Grade unclear if a command is named but the expected output is not.
6. **comment-fits-thread** (preferred) — Check the plan comment against repo facts and thread highlights. Pass if the comment does not promise a deadline, does not guarantee a fix, and respects any stated contribution policy. Fail if it contains a delivery promise or contradicts maintainer guidance. Grade unclear if the repo has no stated policy and the comment is borderline.

If a check's evidence is genuinely absent from the package (the plan has no test plan section at all, for example), grade that check fail, not unclear.

## Verdict assembly

1. Collect the grade for each check: pass, fail, or unclear.
2. Apply the verdict rule from rubric.md: accept if every required check passes; preferred checks never change the verdict; unclear counts as fail on required checks.
3. The verdict is accept or reject (binary).
4. In the output, quote the deciding check: the first required check that failed (if verdict is reject), or "all required checks passed" (if accept).
