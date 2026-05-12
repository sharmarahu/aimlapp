# Pricing & Plans

AIML App uses a **credit-based model**. Credits are consumed per AI request — the cost varies by model tier. This gives you fine-grained control over AI spend without per-seat licensing complexity.

---

## Individual Plans

| | Free | Pro | Business | Enterprise |
|---|---|---|---|---|
| **Credits on signup** | 20 | — | — | — |
| **Monthly credits** | 20 | 1,000 | 5,000 | Custom |
| **Models** | Free-tier models | + Pro models | + Business models | All 40+ models |
| **Rate limit** | 10 req/min | 60 req/min | 150 req/min | 300 req/min |
| **RAG sessions** | ✅ | ✅ | ✅ | ✅ |
| **Collaborative rooms** | ✅ | ✅ | ✅ | ✅ |
| **Cross-model verify** | ✅ | ✅ | ✅ | ✅ |
| **Direct messages** | ✅ | ✅ | ✅ | ✅ |
| **API access** | ❌ | ✅ | ✅ | ✅ |
| **Priority support** | ❌ | ❌ | ✅ | ✅ |
| **Dedicated account manager** | ❌ | ❌ | ❌ | ✅ |

---

## Organization Plans

Organizations are provisioned by the platform admin with a custom credit pool. Pricing is negotiated based on:

- Number of seats
- Monthly credit volume
- Contract length
- Required plan tier (determines which models org members can access)

Contact us for a quote: **it.rahul1@gmail.com**

### Organization Credit Pool
Instead of individual plan credits, org members draw from a shared pool:

```
Org Pool: 100,000 credits/month
├── Member A: quota 10,000 credits
├── Member B: quota 10,000 credits
├── Member C: no quota (uses from pool freely)
└── Remaining pool: available to unquota'd members
```

When the pool is exhausted, all org members are blocked until the pool is refilled.

---

## Credit Costs by Model

| Model | Credits / Request |
|---|---|
| Gemini 2.5 Flash | 1 |
| Gemini 2.0 Flash | 2 |
| Gemini 1.5 Flash | 1 |
| GPT-4o Mini | 2 |
| DeepSeek V3 | 2 |
| Claude Haiku 4.5 | 3 |
| Gemini 1.5 Pro | 5 |
| DeepSeek R1 | 5 |
| GPT-4o | 8 |
| GPT-4.1 | 10 |
| Claude Sonnet 4.5 | 10 |
| o1-mini | 15 |
| Claude Opus 4.7 | 20 |
| o3-mini | 20 |

> Credits are only deducted on successful responses. If the LLM call fails for any reason, credits are automatically refunded.

---

## Credit Events

| Event | Effect |
|---|---|
| Successful LLM response | Credits deducted (model-dependent) |
| LLM API error / timeout | Credits refunded automatically |
| Admin top-up | Credits added, email notification sent |
| Plan upgrade | Credits reset to new plan amount |
| Plan expiry | Downgraded to Free on next request |
| Signup | 20 free credits granted immediately |

---

## Billing FAQ

**Q: What happens when I run out of credits?**
Your requests return a 402 error with the `INSUFFICIENT_CREDITS` code. You'll also receive an email notification when credits drop below a low-balance threshold. Contact your admin to top up, or upgrade your plan.

**Q: Do unused credits roll over?**
No — credits reset at the start of each billing period. Credits added by an admin top-up do not expire.

**Q: Can I get a refund on unused credits?**
Credits are non-refundable but can be transferred between org members by a platform admin.

**Q: What counts as a "request"?**
Each message sent to an AI model is one request, regardless of message length or response length. Verification requests (using the Verify feature) also consume credits from the target model.

**Q: Is there a free trial for organizations?**
Yes — contact us and we'll provision a trial org with a credit pool for your team to evaluate the platform.

---

## Volume Discounts

| Monthly Credits | Discount |
|---|---|
| 10,000 – 49,999 | 10% |
| 50,000 – 199,999 | 20% |
| 200,000+ | Contact us |

---

## Contact

For pricing, enterprise quotes, or billing support: **it.rahul1@gmail.com**
