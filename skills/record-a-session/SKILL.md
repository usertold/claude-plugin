---
name: record-a-session
description: Use when someone wants one specific person to try their site and get the recording back — "have a friend test my landing page", "get a recording of a customer trying signup", "check on that recording link", "share that recording", "revoke the link". Works on any website; the participant installs a Chrome extension and needs no UserTold account.
---

# Record a session with one person

Make a recording link for one named person, follow it until the recording arrives, and share or revoke it only when asked. Nothing is installed on the user's site.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai) and stop.

## 1. Agree on what to ask

You need:

- **Website**: the complete URL where the session starts, e.g. `https://example.com/pricing`.
- **Tasks**: one to five short tasks in plain words that state the outcome, not the route ("Find a plan that fits a team of five", not "Click Pricing, then Teams"). If the user has none, UserTold supplies first-impression tasks.
- **Who**: only so you can tell the user what to send. Don't pass the participant's name or email to UserTold.

Show the tasks before creating the link.

## 2. Create the link

- **Existing project** (the site already has a UserTold project, from `projects.list`): call `recordings.create_invitation` with `projectRef` and a `plan` of `{"version": 1, "title": "...", "targetUrl": "...", "steps": [{"instruction": "..."}]}`. Pass a `studyRef` only if the user wants the session attached to a Study.
- **Any other website**: call `organizations.list`, then `recordings.create_website_invitation` with `organizationRef`, `targetUrl`, an optional `title`, and optional `instructions` (an array of task strings). It creates or reuses a private project for that site.

Always set `externalRef` to a stable value you choose for this person and page, such as `pricing-page-customer-1`. If a call fails or its result is unclear, retry with the same `externalRef` so no duplicate link is created. `expiresInDays` defaults to 7, up to 30.

## 3. Hand over the link

UserTold never sends the link. Give the user the `launchUrl` and the plain installation instructions from the result, and tell them:

- The participant needs desktop Chrome and installs the UserTold recording extension. They don't need a UserTold account.
- When they finish, the recording uploads to the user's project on its own.
- The link expires at `expiresAt`.

Keep the result's invitation `id` and `projectRef`: they are the `invitationRef` and `projectRef` for the next steps.

## 4. Follow up

Call `recordings.get_results` with `projectRef` and `invitationRef` when the user asks how it's going. `status` moves through `invited`, `started`, `uploading`, and `completed`. `completed` means the upload is safe; the transcript and analysis may still be processing. Check `interviews.processing_status` with the returned `interviewRef` if they ask.

Once processed, offer the `review-results` skill to read what the person did and said.

## 5. Share or revoke, only when asked

`recordings.update_invitation` changes access. Call it only when the user explicitly asks, and say what will happen first:

- `share`: creates a 30-day viewing link to a completed recording. **Anyone holding the link can watch or embed it.** Sharing again replaces the previous link.
- `unshare`: turns the viewing link off.
- `revoke`: stops the invitation from being used, blocks further uploads, and turns off any viewing link. It does not delete the recording.

## Boundaries

- Creating a link changes only the user's UserTold workspace. Sending it is the user's choice.
- Never share a recording unless the user asks for a viewing link.
- Recordings come from people who chose to take part. Don't use this to record anyone without their knowledge.
