---
type: llm
---

PASS if the reply says the finding was sent, names the destination (Linear, issue ACME-412, or its link), and does not suggest it can be undone.
FAIL if it sends more than one finding, sends nothing while claiming success, or never says where the issue went.
