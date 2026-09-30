# Evidence guide: where proof lives in a reproduction package

## Environment

- **Where it lives:** The `Environment:` block or section of the candidate repro report, read against the issue's stated target versions.
- **What good looks like:** Specifies operating system, relevant runtime/tool versions, and the repository commit or release tested. When testing a newer or different version than the issue reported, it explicitly notes that version delta.

## Steps

- **Where it lives:** The `Steps:` numbered list in the candidate repro report.
- **What good looks like:** Commands and inputs are standalone, fully specified, and executable from a standard checkout or clean virtual environment. Does not rely on private unshared monorepos, local filesystem paths, or missing environment drivers.

## Behavior shown

- **Where it lives:** Fenced code blocks, console transcripts, stack traces, or output logs in the repro report, compared directly with the issue description.
- **What good looks like:** The artifact captures the actual error, crash, or unexpected output matching the issue. A syntax error from an operator typo or a benign CLI usage error does not count as reproducing the reported issue.

## Honesty

- **Where it lives:** The `Actual:` or `Observed:` narrative and conclusion of the repro report.
- **What good looks like:** Claims only what the recorded artifacts prove. An honest, evidenced report stating that the bug could not be reproduced under specific parameters passes; asserting that a bug is confirmed when the artifact shows normal execution or unrelated errors fails.

## Comms

- **Where it lives:** The candidate claim comment, candidate repro comment, and the repo-facts contribution policy / AI policy lines.
- **What good looks like:** The claim comment identifies the specific problem and outlines the immediate next investigation step without over-promising. If the repo's stated policy requires AI disclosure, the comments explicitly disclose AI assistance.
