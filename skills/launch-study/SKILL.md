---
name: launch-study
description: Use when a UserTold study needs to reach participants — installing the UserTold widget on a website, checking that the install works, choosing where the invitation appears, sharing an interview link, or turning collection on, pausing it, or closing it. Also for "why isn't my study showing up?"
---

# Launch a UserTold study

Get a drafted Study in front of real participants: **Install → Verify → Publish**. Every step before publishing is safe to repeat. Publishing changes what visitors see on the user's site, so it always waits for an explicit go-ahead.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai) and stop.

## 1. Find the project and study

Call `projects.list`, then `studies.list` for the project. If there is no Study yet, offer the `plan-study` skill first. Read the Study with `studies.get` so you know its invitation, page placement, and websites.

## 2. Pick the entry path

| Situation | Path |
|-|-|
| The participants use the user's website or web app | **Widget**: install once and the invitation appears on matching pages |
| The participants can be emailed or messaged a link | **Direct link**: set `invitation.presentation_mode: "direct_link"` with `panel.headline` and `panel.cta`, and put at least one origin in `allowedOrigins` (the link is built from the first). `studies.get` then returns `recruitmentUrl`. It works only while the Study is active, and switching the mode revokes it |
| One specific person should record a session on any website | **Recording invitation**: `recordings.create_invitation` (existing project) or `recordings.create_website_invitation` (any site). The participant installs a Chrome extension; no UserTold account needed. `create_website_invitation` needs an `organizationRef` and an `externalRef` you choose; reuse it on retry to avoid duplicates |

Recording invitations are created, not sent. Hand the `launchUrl` to the user; they decide who receives it.

## 3. Install the widget (widget path)

1. Call `projects.get_widget_setup` to get the project's snippet. It looks like `<script async src="https://usertold.ai/v1/widget.js" data-project-key="ut_pub_…"></script>`. The key is public and safe to commit.
2. Install it **once** in the shared layout so it loads on every page. The same snippet serves every Study in the project. There is no per-study install.
   - If you can edit the user's codebase, find the root layout or document template and add the tag there. Follow the framework's own way of adding third-party scripts. In Next.js that is `next/script`, in Rails the application layout, in a plain site the shared `<head>`.
   - Otherwise, give the user the snippet and say where it goes.
3. If the Study has `allowedOrigins`, every origin the widget runs on must be listed, origin only, e.g. `https://app.example.com`. An empty list allows any origin. Change them with `studies.update` if needed.
4. If the site sets a source-allowlist Content-Security-Policy, add the widget origins to the directives it already defines:
   ```text
   script-src https://usertold.ai https://assets.usertold.ai
   connect-src https://usertold.ai wss://usertold.ai https://assets.usertold.ai https://api.openai.com
   ```
   With a nonce-based `script-src`, put the request nonce on the loader tag. The verification step reports what is blocked. Details: https://usertold.ai/docs/widget

## 4. Verify

Ask the user to confirm the change is deployed, then call `projects.verify_widget_installation` with an exact public HTTPS page that should carry the widget. It checks the deployed HTML, the project key, and browser security policies. It does not run the widget or test microphone and screen permissions. Fix what it flags and verify again. A local `http://localhost` page cannot be verified. Deploy first, or use a preview URL that is public over HTTPS.

## 5. Place the invitation

- **Pages**: `visibility.rules` include or exclude pathnames. `subtree` covers descendants, and exclusions win. `null` means every page.
- **Invitation**: `passive` shows a small launcher. `contextual` opens a panel with a headline and call to action. `direct_link` is for shared links. `contextual` and `direct_link` need `panel.headline` and `panel.cta`. Every invitation also needs `launcher.label` and `launcher.icon`, `brand_color.light` (hex), and `placement.desktop`/`placement.mobile`; ask for the user's brand color. Write invitation copy in the user's voice, short and specific about time and purpose.

Show the user the planned placement and copy before saving it with `studies.update`.

## 6. Publish, only on explicit approval

Call `projects.get_widget_setup` and check `setup.can_start_interviews`. If `start_blocker` is set (for example `insufficient_credits`), say so and point to the UserTold dashboard; publishing won't produce interviews until it's cleared.

Summarize what will happen: which pages, what the invitation says, and that interviews are billed by recorded time to their UserTold balance under current pricing once they finish processing. Then ask. Only after a clear yes, call `studies.update` with `status: "active"`.

- `paused` stops new interviews temporarily, and `closed` ends the collection cycle. Neither interrupts an interview already in progress, and both can return to `active`.
- After launch, suggest the user complete one interview themselves through the real entry path. Then check `interviews.list` and `interviews.processing_status` to confirm the recording and transcript arrived.

## Not showing up?

Check in this order:

1. Study `status` is `active`.
2. `allowedOrigins` is empty or includes the page's origin.
3. `visibility` matches the pathname and language.
4. Another active Study wins that page: a more specific rule, a higher `priority`, or a lower `order`. An exact tie shows nothing, and a winning Study that can't start does not fall back to another.
5. `projects.get_widget_setup` shows no `start_blocker`, such as low balance.
6. `projects.verify_widget_installation` passes on that exact URL.
7. The site's CSP allows UserTold.
