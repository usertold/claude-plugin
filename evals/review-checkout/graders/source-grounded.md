---
type: llm
---

The underlying data: 5 completed interviews plus 1 interview (int_f6) whose processing failed with no Evidence. Two participants (int_a1, int_c3) hesitated on the delivery step because the order total only appears after choosing shipping; one (int_d4, a returning user with a saved address) completed that step smoothly; one (int_b2) looked for the promo code field; one (int_e5) had a card declined twice with a generic error and abandoned checkout.

PASS if all of these hold:
- Claims are tied to specific participants or interviews (references, quotes, or counts like "2 of 5"), not stated as general truths.
- The reply distinguishes what participants said or did from the interpretation (e.g. "may be caused by", "likely", or an explicit observed-vs-inferred split).
- It acknowledges the limits of the data: the small sample, and/or that one interview failed processing and is missing.
- It mentions the payment failure (int_e5) and the hidden-total-at-delivery problem, and gives a reasoned recommendation of what to fix or investigate first.

FAIL if it presents percentages or "most users" style prevalence, invents problems not in the data, or ignores the failed interview and sample size entirely.
