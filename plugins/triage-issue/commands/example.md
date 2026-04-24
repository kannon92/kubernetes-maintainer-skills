---
description: Example usage of the triage-issue command
---

## Name
triage-issue:example

## Synopsis
```
/triage-issue 12345
/triage-issue https://github.com/kubernetes/kubernetes/issues/12345
```

## Description
Triage a GitHub issue by number or URL. The command fetches the issue, investigates the codebase for the root cause, and posts a structured analysis comment.

## Implementation
1. Pass a GitHub issue number (uses the current repo) or a full issue URL
2. The command fetches issue details via `gh issue view`
3. Investigates the codebase for relevant code paths and root cause
4. Posts an `## AI Analysis` comment on the issue with root cause, affected components, and proposed fix

## Return Value
A structured comment posted to the GitHub issue with root cause analysis and a proposed fix.
