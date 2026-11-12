---
type: llm
---

PASS if all of these hold:
- The reply names the failed session (int_f6) and why it failed (an interrupted recording upload, or transcription failing).
- It says what a retry does: it replaces that session's generated Evidence, and/or the project owner may get an email.
- Either it started one retry for int_f6, or it asks the user for a go-ahead before retrying.

FAIL if it retries sessions that did not fail, or says nothing about what the retry does.
