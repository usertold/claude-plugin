---
name: review-results
description: Use when someone asks what users said or did in their UserTold interviews — "what did we learn", "summarize the study", "where are users getting stuck", "what should we fix" — or wants interview evidence grouped into findings and handed to GitHub or Linear. Reads the sources first and keeps quotes, observed behavior, and interpretation separate.
---

# Review UserTold results

Turn completed interviews into conclusions the user can trust. The work is to check what the sources support, not to write the most persuasive summary.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai) and stop.

## 1. Know what you have

1. `projects.list`, then `studies.list`, to find the Study. `studies.get_results` returns interview counts, source-linked Evidence, related Findings, and stated uncertainty. Follow its continuation offsets; don't stop at the first page.
2. `interviews.list` with the `studyRef` shows completed, abandoned, and error interviews; filter `processing_status: "failed"` for those whose analysis failed. For any interview without Evidence, check `interviews.processing_status`. Missing Evidence from a failed or unfinished interview is a capture gap, not proof that nothing went wrong. Use `interviews.retry_processing` only when the status shows a durable failure or a 24-hour stall, and only after telling the user: a retry replaces that interview's generated Evidence (manual Evidence is kept) and may email the project owner.
3. State the base: how many interviews completed, with whom, and on which task.

## 2. Read sources, not summaries

For each important claim, open the moment behind it:

- `evidence.list` filters by study, interview, type, and finding. Pass `findingRef: "none"` for evidence not yet grouped. It defaults to the product under test. Use `target_surface: "all"` only when you also need feedback about the interview itself. Pages hold 5 items; keep paging with `offset` until the result says there are no more.
- `evidence.get` returns the quote, observed behavior, interpretation, page, and timestamp.
- `interviews.get_context` returns the transcript and navigation around a timestamp. Read before and after a quote so a clipped sentence doesn't reverse its meaning.
- `interviews.get_artifacts` links the full transcript and recordings. The links expire after five minutes.

Keep three things apart in everything you write:

| Kind | Example |
|-|-|
| **Said** (participant report) | "I thought inviting a teammate was required." |
| **Did** (observed behavior) | Paused on the invite screen for 40 s, then left setup. |
| **Inferred** (interpretation) | The optional invite step may look mandatory. |

Never turn a pause into "the user was confused" without a quote that says so. Look for counter-evidence too: smooth completions, participants who understood the same screen, differences in task or permissions.

## 3. Report

Lead with what the evidence supports, each point tied to a source:

> **Invite step reads as mandatory** (3 of 5 interviews)
> Said: "I thought I had to invite someone before I could continue." (interview `abc`, 4:12)
> Did: two others paused on `/onboarding/invite` and left setup.
> Counter: one participant skipped it without hesitation.
> Unknown: whether the copy or the button layout causes it.

Counts describe these interviews only. Qualitative studies find problems; they don't measure how common a problem is. Don't extrapolate to "most users". Repeated quotes from the same interview are one participant. Extraction confidence is not the probability a fix will work.

## 4. Group into Findings, with care

A Finding is one coherent problem backed by Evidence.

- Check existing Findings first with `findings.list` and `findings.get_evidence` so you extend them rather than duplicate them.
- `findings.create_from_evidence` makes a private **backlog** Finding. Group by the problem, not by shared words: "invite screen looks mandatory" and "invite email never arrives" are two Findings.
- `findings.update` edits a Finding, attaches or detaches Evidence, or changes its status. Set `status: "ready"` only after you have checked its sources, grouping, relevance to the current product, and stated uncertainty.
- `evidence.update` dismisses Evidence that doesn't hold up. A reason is required. Don't dismiss to tidy up.

Creating or editing Findings changes only the user's UserTold workspace. Say what you changed.

## 5. Hand off, only when asked

`findings.send` creates a GitHub or Linear issue with the linked Evidence attached, and **it cannot be recalled**. Send only a reviewed Finding, only after the user explicitly says to send that Finding, and say where it will go. Use the project's configured destination unless the user names one.

`feedback.submit` is for problems with UserTold itself, never the user's product. Use it only with the user's permission, and leave out personal data and participant details.

## If there's nothing yet

If the Study has no completed interviews, say so plainly and point to the next step. If it isn't live, offer the `launch-study` skill. If it is, suggest running one interview yourself to confirm capture works.
