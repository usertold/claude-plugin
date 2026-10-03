---
name: plan-study
description: Use when someone wants a UserTold user interview or usability test drafted for a product question — why trial users stall, where onboarding breaks, why customers churn or switch, whether a concept or pricing page makes sense, what sits behind a feature request. Turns the question into a validated draft Study without activating anything. Not for changing an existing Study's placement or status (that is launch-study).
---

# Plan a UserTold study

Turn one product question into one focused, validated **draft** Study. A Study is the script UserTold's AI interviewer follows with real participants on the user's website.

The UserTold MCP tools must be connected. If they are missing, tell the user to authorize UserTold (`/mcp` in Claude Code, or add the UserTold connector in Claude.ai; it signs in with their UserTold account) and stop.

## 1. Pin down the decision

A good study rests on four things:

- **Decision**: what will they do differently depending on what they learn?
- **Participants**: who has recently been in the situation? UserTold interviews the user's own visitors and customers; it does not recruit.
- **Encounter**: can the participant use the real product during the interview, or is this a conversation about past behavior?
- **Where**: which site and pages, if the interview runs in their product.

If the request already names the product and the behavior to understand, don't stop to interview the user. Draft right away (steps 2–4) using sensible defaults, mark each assumption, and ask the open questions together with the draft. People react to a concrete draft faster than to a questionnaire. Ask first only when you can't tell what the study is about.

One Study answers one question. If the request covers several workflows, propose separate Studies and plan the most important one first.

If the goal is still vague, call `setup.recommend` with the user's goal in their own words, without personal data or secrets. UserTold saves that request for service improvement.

## 2. Choose the shape

| The question is about… | Shape | `talk.research_mode` |
|-|-|-|
| A real task: onboarding, setup, checkout, pricing choice | `speak → observe → talk → speak` | `usability_debrief` |
| How people handle a problem today | 2–4 focused `talk` segments | `discovery` |
| Why someone adopted, kept, or left a tool | `talk` built around one decision story | `jtbd_switch` |
| Whether an idea, mockup, or message is understood | `talk`, interpretation before explanation | `concept_reaction` |

The UserTold templates are good starting points:

- customer discovery: https://usertold.ai/guides/customer-discovery-interview-questions
- onboarding or activation: https://usertold.ai/guides/find-where-users-get-stuck
- churn or switching: https://usertold.ai/guides/learn-why-users-switch
- pricing page: https://usertold.ai/use-cases/pricing-page-research
- concept testing: https://usertold.ai/guides/concept-testing-interview-questions
- feature requests: https://usertold.ai/guides/identify-user-needs

## 3. Write the script

Use StudyScriptV2: `{"version": 2, "goals": [...], "segments": [...]}`. Every segment has a unique `id`, a `title`, and a `mode`.

- **Goals**: 1–5 concrete objectives, each with a stable `id`. "Understand the user" is too broad; "Learn where first-time buyers hesitate during checkout and what they expected instead" works. Assign every goal to at least one `talk` segment through `talk.goals`.
- **`speak`**: short fixed copy in `speak_text`, for a welcome, a task, or a close.
- **`observe`**: a participant-facing `instruction` that states the outcome and never the route ("Create your first report", not "Click Reports, then New"). Put the expected flow, product terms, and likely failure points in `conductor_context`; participants never see it. Always set a `max_duration_s` safety limit. Add `advance_when` only as `url:<substring>` or `action:<selector>` when the product has a clear completion signal; the validator accepts no other form. Participants can always press Done; that is built in, not an `advance_when` value.
- **`talk`**: a `talk.system_prompt` that names the concrete behavior to ask about. Ask about the most recent real occurrence before opinions or hypotheticals, one question at a time, without suggesting fixes. In a debrief after `observe`, set `experimental_capabilities.realtime_analysis: true` so the interviewer can ask about the specific pauses, errors, and backtracking it saw.

Use the user's real product names and terms. Don't invent features, pages, or URLs. When you don't know a completion URL, leave `advance_when` out (participants press Done, and `max_duration_s` bounds the task) and say that a URL rule can be added later.

## 4. Validate, show, then save

1. Call `studies.validate_script` and fix every error it reports.
2. Show the user the complete draft: goals, each segment in plain language, and the JSON. Explain any choice they might disagree with.
3. Save only after the user approves. Find the project with `projects.list` and reuse a fitting one. Create one with `projects.create` (it needs an `organizationRef` from `organizations.list`) only if none fits and the user agrees. Then call `studies.create`. Include `allowedOrigins` if you know the site's origin.

`studies.create` saves a **draft**. It does not put anything in front of participants. Going live is a separate decision; offer the `launch-study` skill for it.

## Boundaries

- Drafting, validating, and reading never change anything. Saving a Study needs the user's approval, and activating one needs a separate, explicit go-ahead.
- Never set `status: "active"` from this skill.
- Leave experimental capabilities other than `realtime_analysis` off unless the user asks for them.
- A script does not prove anything. Results come from the interviews.
