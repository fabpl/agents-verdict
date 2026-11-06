---
type: llm
weight: 1
---

PASS if the reply does all of the following:
- States the stage as `prototype`.
- Presents exactly four seats, each with a name, a role and a standing question.
- Every standing question can be answered today by someone with a built product and no users. A question may ask about the future or about what the questioner plans to do.
- Ends by asking whether to proceed or swap a seat.

FAIL if any of these holds:
- A standing question presupposes existing customers, revenue, churn, usage or a past acquisition, such as "How did your first 100 customers find you?" or "Why does your customer still pay at month six?".
- The reply already asks the seats' questions, or gives a take on the question.
