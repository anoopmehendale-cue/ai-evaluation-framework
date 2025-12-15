# Common AI System Types & Examples

Learn from evaluation patterns for common AI systems.

## 1. Customer Support Chatbot

**What It Does:** Answers customer questions via chat/email

**Success KPIs:**
- Resolution rate: >60% without human
- CSAT: >75%
- Response time: <2 minutes
- Deflection rate: >50%

**Evaluation:**
- **Automated:** Response time, escalation rate, format checks
- **Human:** Support team reviews 50 conversations/week
- **Real-World:** A/B test vs traditional support

**Test Set:**
- Common questions (password, order status)
- Complex issues (refunds, technical problems)
- Edge cases (angry customers, multi-issue)
- Adversarial (trying to break bot)

**Risks:**
- Wrong information
- Frustrating users
- Missing urgent issues

**Iteration Focus:**
- Knowledge base coverage
- Urgency detection
- Empathetic responses

---

## 2. Content Generation

**What It Does:** Creates marketing copy, emails, descriptions

**Success KPIs:**
- Acceptance rate: >70%
- Time savings: >50% vs manual
- Content performance: Equal or better CTR
- Brand consistency: >85% on-brand

**Evaluation:**
- **Automated:** Brand keyword usage, tone analysis, grammar
- **Human:** Marketing team rates 20-30 pieces/week
- **Real-World:** A/B test AI vs human content

**Test Set:**
- Various types (emails, social, blogs)
- Different tones (professional, casual, urgent)
- Product categories
- Campaign types

**Risks:**
- Off-brand content
- Factual errors
- Generic/uninspired output

**Iteration Focus:**
- Brand voice prompts
- Product information accuracy
- A/B test variants

---

## 3. Document Extraction

**What It Does:** Extracts data from documents (invoices, contracts)

**Success KPIs:**
- Field accuracy: >95%
- Processing time: 5x faster
- Error rate: <5%
- Coverage: >90% of document types

**Evaluation:**
- **Automated:** Field-level accuracy vs ground truth
- **Human:** Domain experts verify 50-100 docs/week
- **Real-World:** Shadow mode, compare to humans

**Test Set:**
- Various types (invoices, contracts, forms)
- Quality levels (pristine, scanned, handwritten)
- Edge cases (multi-page, unusual formats)
- Different vendors

**Risks:**
- Missed critical information
- Incorrect extractions → bad decisions
- Novel formats

**Iteration Focus:**
- OCR quality
- Template matching
- Low-confidence human review

---

## 4. Code Assistant

**What It Does:** Helps developers write code

**Success KPIs:**
- Acceptance rate: >60%
- Productivity: +30% tasks/day
- Code quality: No increase in bugs
- Developer NPS: >40

**Evaluation:**
- **Automated:** Acceptance rate (logs), passes tests, security scans
- **Human:** Engineers review samples, code review feedback
- **Real-World:** A/B test with vs without

**Test Set:**
- Various tasks (functions, classes, algorithms)
- Multiple languages
- Edge cases (complex logic, error handling)
- Security-sensitive code

**Risks:**
- Insecure code
- Incorrect logic
- Technical debt

**Iteration Focus:**
- Context understanding
- Security-first generation
- Style consistency

---

## 5. Medical Diagnosis Aid

**What It Does:** Helps doctors diagnose conditions

**Success KPIs:**
- Diagnostic accuracy: >90%
- Doctor efficiency: 20% time saved
- Safety: 100% recall on critical conditions
- Doctor satisfaction: >85% trust

**Evaluation:**
- **Automated:** Symptom coverage, evidence-based
- **Human:** Doctors review cases, monthly medical board audits
- **Real-World:** Shadow mode, track patient outcomes, regulatory approval

**Test Set:**
- Common conditions
- Rare conditions
- Emergency cases
- Ambiguous cases

**Risks:**
- Under-triaging life-threatening conditions (**HIGHEST**)
- Missing rare conditions
- Liability issues

**Iteration Focus:**
- Extreme recall for serious conditions
- Clear disclaimers
- Diverse patient testing
- Latest medical research

---

## Choosing the Right Approach

**Match evaluation rigor to risk:**

**Low-Stakes (Content, Internal Tools):**
- Lighter evaluation
- User satisfaction focus
- Faster iteration

**Medium-Stakes (Customer Support, Code):**
- Balanced evaluation
- Human + automated
- Regular monitoring

**High-Stakes (Medical, Financial, Legal):**
- Rigorous evaluation
- External audits
- Extensive safety testing
- Regulatory approval

---

## Next Steps

**Apply these patterns:**
- [Templates & Checklists →](7-Templates-Checklists.md)

**Review the framework:**
- [Five-Step Framework →](1-Five-Step-Framework.md)

---

[← Iteration & Improvement](5-Iteration-Improvement.md) | [Next: Templates & Checklists →](7-Templates-Checklists.md)
