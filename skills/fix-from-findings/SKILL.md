---
name: fix-from-findings
description: Use when someone wants to act on what users ran into in UserTold — "fix what users got stuck on", "work on the top finding", "what should I change in the code based on the user tests?", "turn this finding into a PR". Reads the moments behind a reviewed Finding, finds the code involved, and proposes a change for approval.
---

# Fix what users got stuck on

Go from a reviewed Finding to a code change the user can trust: read the moments behind it, find the code, propose the smallest change that addresses it, and plan how the next user tests will show whether it worked.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai) and stop.

## 1. Pick the Finding

1. `projects.list` to find the project.
2. `findings.list` with `status: "ready"` lists reviewed Findings. If the user named one, use `search` to find it. If none is ready, say so and offer `review-results` to review the backlog first; don't fix from unreviewed Findings without saying they are unreviewed.
3. Pick the one the user asked for, or suggest the highest-priority one and say why.

## 2. Read the moments behind it

1. `findings.get_evidence` returns the linked Evidence, timestamps, and uncertainty. Check its truncation flags.
2. For each important quote, `interviews.get_context` with the `interviewRef` and `timestamp_ms` shows what happened before and after, including the pages visited.
3. Write down, separately:
   - **Said**: what participants said, quoted.
   - **Did**: what they did, with the page path.
   - **Inferred**: what you think causes it, marked as a guess.

If the Evidence doesn't support a clear cause, say so. Propose a smaller change or a follow-up test instead of a confident fix.

## 3. Find the code

Use the page paths and product terms from the Evidence to find the route, component, copy, or validation involved in the user's repository. If you can't reach the repository, describe what to change and where to look.

## 4. Propose the change

Show the user:

- the Finding and how many sessions back it (counts from these sessions only, never "most users"),
- the files and the change you propose, kept as small as the problem,
- what is still unknown.

Edit files only through the host's normal approval. When writing a commit message or pull request description, keep Said, Did, and Inferred apart and link the Finding.

## 5. Update UserTold, only with a go-ahead

Each of these needs the user's explicit yes:

- `findings.update` with `status: "in_progress"` when work starts, or `"done"` once the change ships. This changes only their UserTold workspace.
- `findings.send` creates a GitHub or Linear issue that **cannot be recalled**. Use it only if the user asks for an issue, name where it will go, and call it once.

## 6. Check the fix with users

Suggest keeping the Study active, or running it again after the change ships, so new sessions show whether people still get stuck at the same place. `/usertold:check-in` lists what came in since.
