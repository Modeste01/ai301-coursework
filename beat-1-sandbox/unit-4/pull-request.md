# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/150

**Branch**

fix/72-verify-hash-format

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Single full run:
- Run 1 (final): 19/20 agreement (bar: 18/20: PASS)
  - `agreement: 19/20 scored items  (bar: 18/20: PASS)`
  - Categories: `clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`

**Package analysis**

Package: `pkg-02`

- Gold label: `accept` (category: `clear-accept`)
- Rubric verdict: `reject`
- Disagreement: yes (the only disagreement in the 20 scored packages; failed on `standards-and-disclosure`)

Explanation: In `pkg-02` (source: Textualize/rich#4208), the author reordered the `save_*` methods cleanly to match the plan and provided before/after reproduction evidence. However, our rubric's `standards-and-disclosure` check strictly required conventional PR template sections and explicit checklist engagement. Because the author's PR description was concise and informal without explicit template section headers, our rubric graded `standards-and-disclosure` as `fail`, leading to a `reject` verdict. The gold label accepted the PR because the repo lacked strict PR template gating and the changelog asks were satisfied. This package analysis illustrates the strict standard our rubric applies to template completeness.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`, check `plan-fidelity`:

"The diff implements the changes authorized in the plan without unannounced additions (unrequested refactors, new flags, extra configs, or unrelated file changes). Planned changes omitted from the diff are acceptable only if explicitly documented as an honest shortfall or deferral in both plan deviation notes and the PR description. The description does not claim fixes or changes absent from the diff."

Why it reads that way: A simple check that merely tests whether the diff touches the expected files allows silent drift to pass undetected when an author slips in unannounced configuration flags or unrelated refactors (such as in `pkg-03` and `pkg-09`). We deliberately crafted this condition to explicitly catch unannounced additions and false description claims while protecting honest shortfalls and scope deferrals (such as `pkg-13` and `pkg-16`), where incomplete implementations are transparently disclosed in both the plan's deviation notes and the PR description.

**Trade-offs**

Our `standards-and-disclosure` check gives up flexibility on repos with informal PR description customs in exchange for guaranteeing strict adherence on repositories with mandatory templates and policies. It successfully caught `pkg-01` (pandas checklist ignored) and `pkg-20` (Ghostty AI disclosure omitted), but it caused the single false reject on `pkg-02` (Textualize/rich), where an otherwise sound PR was penalized for lacking structured template section headers. This is a deliberate trade-off prioritizing compliance on standardized projects over leniency on informal ones.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
