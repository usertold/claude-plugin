---
description: See what's new from your users since you last looked
argument-hint: "[project or study]"
---

Check UserTold for what is new and list it briefly. Change nothing.

If the UserTold MCP tools are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai) and stop.

1. `projects.list`. If `$ARGUMENTS` names a project or Study, use it; otherwise use the only project, or ask which one.
2. `studies.list` for the project. For each active Study, and any Study named in `$ARGUMENTS`, call `studies.get_results` for interview counts and the newest Evidence.
3. `interviews.list` with `status: "completed"` for finished sessions, and with `processing_status: "failed"` for sessions whose analysis failed. For a failed one, `interviews.processing_status` says why. Don't retry it from here.
4. `evidence.list` with `findingRef: "none"` for moments not yet grouped into a Finding. Read the first page only, and say when `truncated` shows there are more.
5. `findings.list` with `status: "ready"` for reviewed Findings waiting for work.

If this conversation already ran a check-in, report only what changed since then. Otherwise report the current state.

Reply in at most ten lines:

- finished sessions, per Study,
- moments not yet grouped,
- failed processing, with the reason,
- ready Findings, by title,
- one suggested next step: `review-results` for new moments, `fix-from-findings` for a ready Finding, or nothing to do.
