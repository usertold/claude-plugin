# UserTold for Claude

Plan AI-led user interviews and usability tests, put them live on your website, and turn what participants said and did into reviewed findings.

[UserTold](https://usertold.ai) runs voice interviews with your own users inside your website. It records the conversation and page activity, plus the screen on desktop browsers where the participant allows it. With consent, it can watch a participant try a task without giving hints, then ask about the specific moments it saw. Each answer stays linked to the recording that prompted it.

This plugin connects Claude to your UserTold account. It also teaches Claude how to plan a study, put it live, and review the results, keeping quotes, observed behavior, and interpretation separate.

## What's inside

| Component | What it does |
|-|-|
| **UserTold connector** | Remote MCP server at `https://mcp.usertold.ai/mcp`. You sign in with your UserTold account over OAuth. |
| **plan-study** skill | Turns a product question into one focused draft study. It chooses between a watched task and a conversation, writes neutral questions, and validates the script before you approve saving it. |
| **launch-study** skill | Installs the widget snippet once in your site's shared layout, verifies a deployed page, sets where the invitation appears, and turns collection on only after you say so. |
| **review-results** skill | Reads interview sources before summaries. It groups evidence (a quote or observed moment, linked to its recording) into findings (one problem backed by several moments), and sends a finding to GitHub or Linear only when you ask. |

## Getting started

1. Install the plugin and connect the UserTold connector when prompted. Sign up at [usertold.ai](https://usertold.ai/login) if you don't have an account.
2. Ask a question about your users, for example:
   - "Why do trial users stop before creating their first report? Set up an interview to find out."
   - "Put the onboarding study live on app.example.com."
   - "What did we learn from last week's checkout interviews? What should we fix first?"

## What it accesses and sends

This plugin contains skills (plain-text instructions), a reference to the UserTold MCP server, an icon, and eval fixtures with mock data. It runs no code on your machine and bundles no packages.

- **Connector traffic.** All tool calls go to UserTold at `https://mcp.usertold.ai/mcp`, under your UserTold account and its permissions. They read and change the projects, studies, interviews, evidence, and findings in your UserTold workspace.
- **Setup guidance.** When Claude asks UserTold for research setup guidance (`setup.recommend`), UserTold saves that request and its response to improve the service. Leave personal data and secrets out of research goals.
- **Your website.** `launch-study` may edit your codebase to add the UserTold `<script>` tag (a public project key). To check the install, it asks UserTold to fetch one public page of your site (`projects.verify_widget_installation`); the plugin itself makes no requests.
- **Templates.** `plan-study` may read public guide pages on usertold.ai for script templates. Nothing about your project is sent in those requests.
- **Third parties.** Nothing goes to GitHub or Linear unless you explicitly send a finding (`findings.send`). That uses the integration configured in your UserTold project and creates an issue that can't be recalled. A recording is shared only when you explicitly create a viewing link.
- **Feedback.** Claude submits a bug report or suggestion to the UserTold team (`feedback.submit`) only with your permission.

Activating a study puts an interview invitation in front of your site's visitors. Interviews are billed by recorded time to your UserTold balance under the [current pricing](https://usertold.ai/pricing), once an interview finishes processing with a recording and transcript. Abandoned or failed interviews aren't charged. The skills never activate a study without your explicit go-ahead.

Participant recordings and transcripts are processed under the UserTold [Privacy Policy](https://usertold.ai/privacy) and [Terms](https://usertold.ai/terms).

## Support

- Documentation: https://usertold.ai/docs/mcp
- Support: https://usertold.ai/support
- Issues with this plugin: https://github.com/usertold/claude-plugin/issues

## License

MIT
