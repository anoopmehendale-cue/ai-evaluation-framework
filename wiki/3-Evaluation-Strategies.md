# Evaluation Strategies

How to systematically test AI systems using a three-tier approach.

## The Three-Tier Strategy

### Tier 1: Automated Metrics
**Fast, scalable, continuous**

**What:** Measurements that run automatically
**When:** Development, testing, production
**Cost:** Low (after setup)

**Examples:**
- Response time from logs
- Format validation (JSON, required fields)
- Keyword presence/absence
- LLM-as-judge scores
- Consistency checks

**Best For:**
- High-volume testing
- Continuous monitoring
- Regression testing
- Quick feedback cycles

**Limitations:**
- Can't capture nuanced quality
- May miss edge cases
- Doesn't reflect user perception

---

### Tier 2: Human Evaluation
**Nuanced, contextual, reliable**

**What:** Human reviewers assess quality
**When:** Pre-launch, spot checks, investigations
**Cost:** Medium to high

**Who Reviews:**
- Domain experts (correctness)
- End users (usefulness)
- QA team (consistency)

**Methods:**
- Rubrics (1-5 with criteria)
- Comparative (A vs B)
- Binary (pass/fail)
- Open feedback

**Sample Sizes:**
- Exploratory: 20-50
- Pre-launch: 100-200
- Ongoing: 50-100/week

**Best For:**
- Subjective quality
- Complex outputs
- High-stakes decisions
- Understanding failures

---

### Tier 3: Real-World Validation
**True user impact, business outcomes**

**What:** Test with actual users
**When:** Pre-launch (pilot), post-launch (monitoring)
**Cost:** High (risk + resources)

**Approaches:**
- A/B testing (AI vs baseline)
- Shadow mode (run parallel, don't act)
- Pilot program (limited users)
- Phased rollout (1% → 10% → 100%)

**What to Measure:**
- Business KPIs
- User behavior
- Feedback/satisfaction
- Adoption patterns

**Best For:**
- Business impact validation
- User experience assessment
- Risk mitigation before full launch

---

## Test Set Design

Build a comprehensive test set:

### 1. Happy Path (60%)
Common, straightforward cases that should work perfectly.

**Example (Chatbot):**
- Password reset
- Order status check
- Basic FAQ

### 2. Edge Cases (20%)
Unusual but valid inputs.

**Example (Document Extraction):**
- Poor scan quality
- Unusual formatting
- Multi-language

### 3. Stress Tests (10%)
System limits.

**Example (Meeting Assistant):**
- 4-hour meeting
- 30 participants
- Heavy background noise

### 4. Adversarial (5%)
Deliberately tricky inputs.

**Example (Content Moderator):**
- Code words
- Sarcasm
- Context-dependent meaning

### 5. Regression (5%)
Previously fixed bugs.

**Example:**
- User-reported failures
- Known edge cases
- Critical past mistakes

---

## Choosing the Right Mix

**Early Development:**
- 80% Tier 1 (automated)
- 20% Tier 2 (human spot checks)

**Pre-Launch:**
- 40% Tier 1 (regressions)
- 40% Tier 2 (quality validation)
- 20% Tier 3 (pilot with real users)

**Post-Launch:**
- 60% Tier 1 (continuous monitoring)
- 20% Tier 2 (weekly audits)
- 20% Tier 3 (A/B tests, metrics)

---

## Evaluation Signals

Where to collect data:

**User Feedback:**
- Surveys (CSAT, NPS)
- Ratings (thumbs, 1-5 stars)
- Support tickets
- Feature requests

**System Logs:**
- Errors and exceptions
- Latency/performance
- Usage patterns
- User actions (accept/reject)

**Business Metrics:**
- Revenue, conversions
- Cost savings
- Retention, churn

**Expert Review:**
- Sample evaluations
- Audits
- Deep dives on failures

**Baselines:**
- Current system
- Human performance
- Competitors

---

## Next Steps

**Plan your evaluation:**
- [Stakeholder Involvement →](4-Stakeholder-Involvement.md)
- [Iteration & Improvement →](5-Iteration-Improvement.md)
- [Templates & Checklists →](7-Templates-Checklists.md)

---

[← Success Metrics](2-Success-Metrics.md) | [Next: Stakeholder Involvement →](4-Stakeholder-Involvement.md)
