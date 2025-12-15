# Defining Success Metrics

"Good" is subjective. Make it concrete with specific, measurable criteria.

## Why Success Metrics Matter

Without clear metrics:
- ❌ Endless debates about "good enough"
- ❌ Misalignment between teams
- ❌ Optimizing for the wrong things
- ❌ Uncertain launch decisions

With clear metrics:
- ✅ Objective launch criteria
- ✅ Clear improvement targets
- ✅ Aligned stakeholders
- ✅ Measurable progress

---

## Four Categories of Metrics

### 1. Business Metrics (Highest Priority)

**These directly impact the bottom line:**
- Revenue (sales, upsells, conversions)
- Cost savings (labor, time, resources)
- User satisfaction/retention (NPS, CSAT, churn)
- Process efficiency (throughput, cycle time)

**Example:**
- Chatbot → Reduce support costs by 40%
- Document extraction → 5x faster processing

**Rule of Thumb:** Your primary KPI should be a business metric.

---

### 2. Quality Metrics

**Technical quality of outputs:**
- **Accuracy:** % correct overall
- **Precision:** When it says "yes," is it right?
- **Recall:** Does it catch all important cases?
- **Consistency:** Same input → same output?
- **Relevance:** Is output useful?

**Example:**
- Medical diagnosis → 99% recall on serious conditions
- Spam filter → 95% precision on spam

**When to Use:** As supporting metrics for business goals.

---

### 3. User Experience Metrics

**How users perceive the system:**
- Response time/latency
- Completion rate (finish what they started?)
- User satisfaction (ratings, feedback)
- Adoption rate (are people using it?)
- Error recovery (can they fix mistakes?)

**Example:**
- Code assistant → 70% acceptance rate
- Content generator → <30% need major edits

**When to Use:** When user adoption is critical to business value.

---

### 4. Risk Metrics

**What could go wrong:**
- False positive rate (wrong yes)
- False negative rate (missed important cases)
- Harmful output rate
- Bias/fairness (disparate impact)
- Safety violations

**Example:**
- Resume screener → Demographic parity in pass rates
- Content moderator → <1% false negative on harm

**When to Use:** For high-stakes systems or regulated industries.

---

## Setting Quality Bars

For each metric, define minimum acceptable thresholds:

**Example: Customer Support Chatbot**

```
SUCCESS DEFINITION:

Primary KPI: Reduce response time from 4hr to <1hr
Target: 80% of inquiries responded to within 1 hour

Quality Bars:
- Categorization accuracy: >90%
- Urgency detection: >95% (critical)
- Response acceptance: >70%

User Experience:
- User satisfaction (CSAT): >75%
- Completion rate: >85%

Deal-Breakers:
- Missing urgent issues
- Inappropriate responses
- Making agents slower

Nice-to-Haves:
- Multi-language support
- Sentiment-aware responses
```

---

## Common Mistakes

**❌ Only tracking accuracy**
- Accuracy doesn't capture business value
- 95% accurate but users hate it = failure

**✅ Start with business KPIs, add quality metrics as needed**

---

**❌ Setting unrealistic bars**
- 99% accuracy on everything
- Perfection paralysis

**✅ Define "good enough" based on use case and risk**

---

**❌ Too many metrics**
- Tracking 20 things = tracking nothing
- Analysis paralysis

**✅ 1 primary KPI + 3-5 supporting metrics**

---

**❌ Metrics you can't measure**
- "User delight" without definition
- Abstract goals

**✅ Specific, measurable, time-bound**

---

## Template: Success Definition

```
SYSTEM: [Name of AI system]

PRIMARY KPI: [The #1 business metric]
Target: [Specific number with timeframe]

QUALITY BARS:
- [Metric 1]: [Threshold]
- [Metric 2]: [Threshold]
- [Metric 3]: [Threshold]

USER EXPERIENCE:
- [Metric]: [Threshold]
- [Metric]: [Threshold]

DEAL-BREAKERS:
- [What would make this unacceptable]
- [Critical failure modes to avoid]

NICE-TO-HAVES:
- [Secondary goals]
- [Future improvements]
```

---

## Examples by System Type

### Chatbot
- **Primary:** Resolution rate >60%
- **Quality:** Response accuracy >85%
- **UX:** CSAT >75%
- **Risk:** Missing urgent issues <1%

### Content Generation
- **Primary:** Time savings >50%
- **Quality:** Acceptance rate >70%
- **UX:** Edit time <5 min
- **Risk:** Off-brand content <5%

### Document Extraction
- **Primary:** Processing time 5x faster
- **Quality:** Field accuracy >95%
- **UX:** Manual review rate <10%
- **Risk:** Critical errors <0.1%

### Code Assistant
- **Primary:** Productivity +30%
- **Quality:** Acceptance rate >60%
- **UX:** Developer NPS >40
- **Risk:** Security issues <1%

---

## Next Steps

Once you've defined success:

1. **[Design Evaluation →](3-Evaluation-Strategies.md)** - Plan how to measure these metrics
2. **[Involve Stakeholders →](4-Stakeholder-Involvement.md)** - Align on success criteria
3. **[Templates & Checklists →](7-Templates-Checklists.md)** - Use ready-made templates

---

[← Five-Step Framework](1-Five-Step-Framework.md) | [Next: Evaluation Strategies →](3-Evaluation-Strategies.md)
