---
name: get-started
description: Use when someone wants feedback from their users, wants to set up or connect UserTold, asks what UserTold can do for their product, or isn't sure where to start — for example "I want to know why people drop off", "set up UserTold on my site", "how do I see what users do with this?"
---

# Get started with UserTold

Find out what the user needs, then hand off to the skill that does it. Keep this short: one tool call for guidance, one to find their account, then the next step.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai; it signs in with their UserTold account) and stop.

## 1. Ask UserTold first, in the user's words

Call `setup.recommend` with `request` set to what the user asked for, as close to their own words as possible. Don't narrow it to a research method or rewrite it into jargon. Remove personal data, customer names, and secrets first: UserTold saves the request and its response to improve the service.

Use the guidance it returns to shape the next step. Don't paste it back whole.

## 2. Find where they are

Call `projects.list`.

- **No projects**: call `organizations.list`. Say that a project for their site comes later, when they save a first test or make a recording link. Don't create one yet.
- **One or more projects**: name the one that fits their site. If several could fit, ask which.

If they already have Studies, `studies.list` shows which are draft, active, paused, or closed.

## 3. Route

| They want to… | Next |
|-|-|
| Understand a specific behavior on their own site or app ("why do trial users stop before…") | `plan-study` drafts a test |
| Put a drafted test in front of visitors, or fix "my test isn't showing up" | `launch-study` |
| See one specific person (a friend, a customer) try a page, on any website | `record-a-session` |
| Know what came back from tests already run | `review-results` |
| Fix what users got stuck on | `fix-from-findings` |

Say in one or two sentences what will happen next, then continue with that skill. If the request is still open ("I just want feedback"), suggest the smallest useful start: one test on one important flow, or one recording link sent to one customer.

## Boundaries

- This skill changes nothing in UserTold. Creating projects, saving tests, and turning them on happen in the skills above, each with the user's approval.
- If the user asks about something UserTold doesn't do, such as surveys or recruiting strangers, say so plainly and don't call UserTold tools for it.
