# UserTold for Claude

See what users do on your site and hear what they say, then fix what gets in their way with Claude.

[UserTold](https://usertold.ai) runs a short user test on your website. It records the page, and the screen and microphone where the participant allows, so what people did and what they said stay together. A voice conversation before or after the task is optional. Each moment links back to its recording.

This plugin connects Claude to your UserTold account. It teaches Claude to plan a test, put it live, read what came back, and turn it into changes you and your agents can make, keeping what people said, what they did, and what Claude infers apart.

## What's inside

| Component | What it does |
|-|-|
| **UserTold connector** | Remote MCP server at `https://mcp.usertold.ai/mcp`. You sign in with your UserTold account over OAuth. |
| **get-started** skill | For "I want feedback from my users" or "set up UserTold". Asks UserTold for setup guidance in your own words, finds your project, and points to the next step. |
| **plan-study** skill | Turns a product question into one focused draft test: a task the participant tries, neutral questions, and a script that is validated before you approve saving it. |
| **launch-study** skill | Adds the UserTold script once to your site's shared layout, checks a deployed page, sets where the invitation appears, and turns the test on only after you say so. |
| **record-a-session** skill | Makes a recording link for one specific person, such as a friend or a customer, on any website. You decide who gets the link. |
| **review-results** skill | Reads the recordings and transcripts before summaries, groups repeated problems into findings, and sends a finding to GitHub or Linear only when you ask. |
| **fix-from-findings** skill | Takes a reviewed finding, reads the moments behind it, finds the code involved, and proposes a fix that you approve. |
| **check-in** command | `/usertold:check-in` lists what is new since you last looked: finished tests, moments not yet grouped, processing failures, and findings ready to work on. |

## Getting started

1. Install the plugin.
   - In Claude Code: `/plugin install usertold --marketplace usertold/claude-plugin` (Claude Code 2.1.275 or later). On older versions, run `/plugin marketplace add usertold/claude-plugin`, then `/plugin install usertold@usertold`.
   - On claude.ai and in Cowork: once the plugin is listed in the Claude directory, add it from **Customize > Plugins**.
2. Connect the UserTold connector when prompted and sign in. Sign up at [usertold.ai](https://usertold.ai/login) if you don't have an account.
3. Ask about your users, for example:
   - "Why do trial users stop before creating their first report? Set up a user test to find out."
   - "Put the onboarding test live on app.example.com."
   - "Get a recording of a customer trying our pricing page."
   - "What did people run into in last week's checkout tests? What should we fix first?"
   - "Fix the top finding."

In Claude Code you can repeat the check-in while you work, for example `/loop 2h /usertold:check-in`.

## What it accesses and sends

This plugin contains skills and a command (plain-text instructions), a reference to the UserTold MCP server, a marketplace file, an icon, and eval fixtures with mock data. It runs no code on your machine and bundles no packages.

- **Connector traffic.** All tool calls go to UserTold at `https://mcp.usertold.ai/mcp`, under your UserTold account and its permissions. They read and change the projects, studies, interviews, recordings, evidence, and findings in your UserTold workspace.
- **Setup guidance.** When Claude asks UserTold for setup guidance (`setup.recommend`), UserTold saves that request and its response to improve the service. Leave personal data and secrets out of what you ask for.
- **Your website.** `launch-study` may edit your codebase to add the UserTold `<script>` tag (a public project key). To check the install, it asks UserTold to fetch one public page of your site (`projects.verify_widget_installation`); the plugin itself makes no requests. The widget it installs runs in your visitors' browsers. It connects to usertold.ai. When the participant turns on the mic (think-aloud or the optional voice conversation), their browser streams the audio directly to OpenAI's realtime API for transcription and, in a voice conversation, for spoken replies. Both are named in the UserTold [Privacy Policy](https://usertold.ai/privacy). The plugin itself sends nothing there.
- **Recording links.** `record-a-session` creates a link; it never sends it to anyone. The participant installs the UserTold Chrome extension and needs no UserTold account. A recording is shared only when you explicitly ask for a viewing link, and anyone holding that link can watch it for 30 days unless you revoke it.
- **Your code.** `fix-from-findings` reads and edits files in your repository only through Claude's normal file tools and approvals. It sends no code to UserTold.
- **Templates.** `plan-study` may read public guide pages on usertold.ai for script templates. Nothing about your project is sent in those requests.
- **Third parties.** Nothing goes to GitHub or Linear unless you explicitly send a finding (`findings.send`). That uses the integration configured in your UserTold project and creates an issue that can't be recalled.
- **Feedback.** Claude submits a bug report or suggestion to the UserTold team (`feedback.submit`) only with your permission.

Turning a test on puts an invitation in front of your site's visitors. User tests are billed by recorded time to your UserTold balance under the [current pricing](https://usertold.ai/pricing), once a session finishes processing with a recording and transcript. Abandoned or failed sessions aren't charged. The skills never turn a test on without your explicit go-ahead.

Participant recordings and transcripts are processed under the UserTold [Privacy Policy](https://usertold.ai/privacy) and [Terms](https://usertold.ai/terms).

## Evals

The `evals/` directory holds cases for `claude plugin eval`. Mocks answer every UserTold tool call, so no real account is touched. Run them from the plugin root and keep the report local:

```bash
claude plugin eval . --no-publish --runs 3 -j 2
```

Without `--no-publish`, the report is published to claude.ai when your account supports it. `evals/mocks/usertold/_tools.json` is a saved `tools/list` response; refresh it whenever the server's tool titles, descriptions, or schemas change.

## Support

- Documentation: https://usertold.ai/docs/mcp
- Support: https://usertold.ai/support
- Issues with this plugin: https://github.com/usertold/claude-plugin/issues

## License

MIT
