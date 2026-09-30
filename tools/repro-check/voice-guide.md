# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor working primarily in Python on backend bug fixes and unit testing. I communicate plainly, report only what I have verified through reproduction, and promise only my immediate investigation.

## Rules I write by

### Rule: promise-investigation-not-fixes

Promise your investigation and testing, never a guaranteed fix or an arbitrary deadline.

- Wrong: "I will fix this bug by tomorrow and submit a pull request."
- Right: "I'd like to investigate this issue and see if I can reproduce the unexpected exception locally."

### Rule: ground-claims-in-artifacts

State only what your captured logs and tests demonstrate, without generalizing beyond your evidence.

- Wrong: "I ran the script and can confirm this completely breaks password authentication everywhere."
- Right: "Running `verify_password` against a malformed hash string raised `UnknownHashError` rather than returning `False`."

### Rule: state-environment-and-deltas

Always list your operating system, Python version, and dependency versions, explicitly calling out any differences from the issue.

- Wrong: "Reproduced on my machine with latest dependencies."
- Right: "Tested on Ubuntu 24.04 (WSL2), Python 3.12, passlib 1.7.4, bcrypt 4.3.0, and pytest 9.1.1 on clean main at commit f89c06f."

### Rule: disclose-ai-usage-when-required

If the repository policy requires disclosing AI assistance, state it clearly and factually.

- Wrong: (Submitting AI-assisted reports without mentioning it in repos that mandate disclosure)
- Right: "Per the project's AI policy, I used an AI assistant to help organize this reproduction report; all commands and outputs were executed and verified by me."

## Things I never post

- Generic "+1" or "Can I work on this?" comments without technical context or intended next steps.
- Delivery deadlines or promises of guaranteed fixes.
- Speculative root-cause diagnoses before inspecting the codebase.
