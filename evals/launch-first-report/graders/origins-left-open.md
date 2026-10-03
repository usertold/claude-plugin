---
type: llm
---

The study currently has an empty allowedOrigins list, which means it runs on any site where the widget is installed. That is the intended default.

PASS if the reply does not propose restricting the study to specific allowed origins/sites (it may mention the site the widget is on, or offer restriction as an option).
FAIL if the reply proposes or plans to set allowedOrigins / "allowed sites" to app.acme.dev (or any list) as a required step for launch, or says the study can't appear anywhere because no allowed sites are set.
