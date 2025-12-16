# AI Systems: Design & Evaluation Frameworks

This repository contains two comprehensive frameworks for building successful AI systems:

## 📊 [AI Evaluation Framework](README.md)
How to evaluate AI systems before and after deployment
- **[Full Documentation](README.md)** - Comprehensive evaluation guide
- **[Wiki](wiki/Home.md)** - Quick reference pages
- **Focus:** Measuring success, defining metrics, continuous improvement

## 🏗️ [Compound AI Systems Framework](README_COMPOUND_AI_SYSTEMS.md)
How to design and build multi-agent AI systems
- **[Full Documentation](README_COMPOUND_AI_SYSTEMS.md)** - Complete implementation guide
- **[Wiki](wiki-compound/Compound-AI-Home.md)** - Step-by-step learning pages
- **Focus:** Architecture patterns, agent design, orchestration

---

# AI Evaluation Framework

## Introduction

Evaluating AI systems is fundamentally different from evaluating traditional software. AI systems are probabilistic, context-dependent, and often involve subjective quality judgments. Without a rigorous evaluation framework, teams risk:

- **Deploying systems that don't deliver business value** - High accuracy on test sets doesn't guarantee real-world impact
- **Missing critical failure modes** - Edge cases and adversarial inputs that weren't anticipated
- **Losing stakeholder trust** - When systems fail in production, especially in high-stakes scenarios
- **Wasting resources** - Iterating blindly without systematic feedback loops

This framework provides a structured approach to evaluating AI systems across five key dimensions: understanding the system, defining success, designing evaluation strategies, involving stakeholders, and planning for iteration.

**Who This Is For:**
- Product managers launching AI features
- ML engineers building AI systems
- Business leaders overseeing AI initiatives
- QA teams responsible for AI quality assurance
- Anyone evaluating whether an AI system is ready for production

**When to Use This Framework:**
- Before launching a new AI feature
- When migrating from manual processes to AI
- During system redesigns or major updates
- When investigating production issues
- For ongoing monitoring and improvement

---

## The 5-Step Evaluation Framework

### Step 1: Understand the System

Before evaluating anything, deeply understand what you're evaluating.

#### Key Questions:

**System Function:**
- What is the AI system's primary function?
- What problem is it solving?
- What does it replace (manual process, older system, nothing)?

**Users & Context:**
- Who are the end users? (customers, employees, domain experts)
- What's their current workflow?
- How does this fit into their broader process?

**Business Value:**
- What's the business goal? (revenue, efficiency, satisfaction)
- What's the expected ROI?
- Why now? What's the urgency?

**Risk Profile:**
- What happens if it fails?
- Is this high-stakes (medical, financial) or low-stakes?
- Are there regulatory or compliance considerations?

**Technical Details:**
- What are the inputs and outputs?
- Is it customer-facing or internal?
- Real-time or batch processing?
- Autonomous or human-in-the-loop?

#### System Profile Template:

```
SYSTEM PROFILE:
Function: [One sentence: what it does]
Users: [Who uses it, how often]
Business Value: [Why it matters, expected impact]
Risk Level: [High/Medium/Low with justification]
Input/Output: [What goes in, what comes out]
Workflow Integration: [How it fits into existing processes]
Success Dependency: [What needs to be true for this to work]
```

**Example:**
```
SYSTEM PROFILE:
Function: AI chatbot that handles customer support inquiries for an e-commerce platform
Users: Customers with questions (2M inquiries/month), support team (escalations)
Business Value: Reduce support costs by 40%, improve response time from 8min to <1min
Risk Level: Medium - Wrong info frustrates customers but not life-threatening
Input/Output: Customer text questions → Answers + action (resolve or escalate)
Workflow Integration: First line of defense, escalates to human agents when needed
Success Dependency: Must maintain >75% customer satisfaction, handle 60% of inquiries autonomously
```

---

### Step 2: Define Success

"Good" is subjective. Make it concrete with specific, measurable criteria.

#### Success Criteria Categories:

**A. Business Metrics (Highest Priority)**

These directly impact the bottom line:
- **Revenue impact:** Direct sales, upsells, conversions
- **Cost savings:** Labor, time, resources
- **Time savings:** Process efficiency, time-to-value
- **User satisfaction/retention:** NPS, CSAT, churn
- **Business process efficiency:** Throughput, cycle time

**Example:**
- Customer support chatbot → Primary KPI: 40% reduction in support costs
- Document extraction → Primary KPI: 5x faster processing vs manual

**B. Quality Metrics**

Technical quality of outputs:
- **Accuracy:** How often is it correct overall?
- **Precision:** When it says "yes," is it right? (minimize false positives)
- **Recall:** Does it catch all important cases? (minimize false negatives)
- **Consistency:** Same input → same output?
- **Relevance:** Is the output useful for the user's goal?

**Example:**
- Medical diagnosis aid → Recall on serious conditions must be 99%+ (can't miss)
- Spam filter → Precision must be 95%+ (can't flag legitimate emails)

**C. User Experience Metrics**

How users perceive and interact with the system:
- **Response time/latency:** How fast does it respond?
- **Completion rate:** Do users finish what they started?
- **User satisfaction:** Ratings, feedback, complaints
- **Adoption rate:** Are people actually using it?
- **Error recovery:** When it fails, can users fix it?

**Example:**
- Code assistant → Acceptance rate: 70%+ of suggestions used
- Content generator → Edit rate: <30% of outputs need major changes

**D. Risk Metrics**

What could go wrong:
- **False positive rate:** Says "yes" when should say "no"
- **False negative rate:** Misses important cases
- **Harmful output rate:** Offensive, dangerous, or incorrect content
- **Bias/fairness:** Disparate impact across demographic groups
- **Safety violations:** Breaches of safety guidelines or policies

**Example:**
- Resume screener → Must maintain demographic parity in pass rates
- Content moderator → False negative rate on harmful content <1%

#### Success Definition Template:

```
SUCCESS DEFINITION:
Primary KPI: [The #1 business metric that matters]
Quality Bar: [Minimum acceptable quality thresholds]
User Experience: [What good UX looks like]
Deal-Breakers: [What would make this unacceptable]
Nice-to-Haves: [Secondary goals, aspirational targets]
```

**Real-World Example: Email Triage Assistant**

```
SUCCESS DEFINITION:
Primary KPI: Reduce support team response time from 4 hours to <1 hour
Quality Bar:
  - Categorization accuracy: >90%
  - Urgency detection: >95% (can't miss critical issues)
  - Draft response acceptance: >70%
User Experience:
  - Agents find it helpful, not frustrating
  - Reduces workload, doesn't create more work
  - Intuitive overrides when AI is wrong
Deal-Breakers:
  - Missing urgent issues (security, payment failures)
  - Suggesting inappropriate responses
  - Making agents slower (bad UX, poor accuracy)
Nice-to-Haves:
  - Multi-language support
  - Sentiment analysis
  - Auto-response for simple cases
```

---

### Step 3: Design Evaluation

How do you actually test if the system meets your success criteria?

#### Three-Tier Evaluation Strategy:

**TIER 1: Automated Metrics**

What can be measured automatically, at scale:

**Advantages:**
- Fast, cheap, scalable
- Consistent, reproducible
- Can run continuously

**Common Approaches:**
- **LLM-as-judge:** Use another LLM to evaluate outputs
- **Format validation:** Check structure, JSON schema, required fields
- **Keyword/pattern matching:** Presence/absence of specific terms
- **Length checks:** Too short, too long, within bounds
- **Sentiment analysis:** Tone, politeness, formality
- **Factual consistency:** Compare to source documents
- **Similarity metrics:** Compare to reference outputs

**When to Use:**
- High-volume testing (thousands of cases)
- Continuous monitoring in production
- Quick feedback during development
- Regression testing

**Limitations:**
- Can't capture nuanced quality
- May miss edge cases
- Doesn't reflect user perception

**Example:**
```
Automated Metrics for Customer Support Chatbot:
- Response time (measured from logs)
- Escalation rate (% passed to humans)
- Format validation (has greeting, answer, closing)
- Sentiment score (politeness check)
- Reference check (answer matches knowledge base)
```

**TIER 2: Human Evaluation**

What requires human judgment:

**Who Should Evaluate:**
- **Domain experts:** For correctness, technical accuracy
- **End users:** For usefulness, user experience
- **Internal QA team:** For consistent quality checks

**Evaluation Methods:**
- **Rubrics:** Detailed scoring criteria (1-5 scale with definitions)
- **Comparative evaluation:** A vs B, which is better?
- **Likert scales:** Rate from strongly disagree to strongly agree
- **Binary judgment:** Good/bad, pass/fail
- **Open feedback:** What's wrong? How to improve?

**Sample Size Guidelines:**
- **Exploratory:** 20-50 examples
- **Pre-launch:** 100-200 examples
- **Ongoing monitoring:** 50-100 examples/week

**Example Rubric:**
```
Email Response Quality (1-5 scale):

5 - Excellent: Accurate, helpful, on-brand, needs no edits
4 - Good: Accurate and helpful, minor edits needed
3 - Acceptable: Correct but could be more helpful, moderate edits
2 - Poor: Partially correct, major edits needed
1 - Unacceptable: Incorrect, unhelpful, or inappropriate

Evaluate on 3 dimensions:
- Factual accuracy
- Helpfulness/completeness
- Tone and professionalism
```

**TIER 3: Real-World Validation**

Testing with actual users and business outcomes:

**Approaches:**
- **A/B testing:** Compare AI system vs baseline (control group)
- **Shadow mode:** Run AI in parallel, don't act on output, compare to humans
- **Pilot program:** Limited rollout to subset of users
- **Beta testing:** Opt-in for early access
- **Phased rollout:** Gradual expansion (1% → 5% → 25% → 100%)

**What to Measure:**
- Business metrics (the ones you defined in Step 2)
- User behavior (adoption, retention, completion rates)
- Feedback (surveys, support tickets, social media)

**Best Practices:**
- Start small (minimize risk)
- Monitor closely (daily metric reviews)
- Have rollback plan (can revert quickly)
- Collect qualitative feedback (not just numbers)

#### Test Set Design:

A comprehensive test set covers multiple scenarios:

**1. Happy Path Cases (60% of test set)**
- Common, straightforward inputs
- Expected use cases
- Should work flawlessly

**Example (Code Assistant):**
- Write a function to sort a list
- Create a class with constructor
- Basic string manipulation

**2. Edge Cases (20% of test set)**
- Unusual but valid inputs
- Boundary conditions
- Ambiguous situations

**Example (Document Extraction):**
- Scanned documents with poor quality
- Documents with unusual formatting
- Multi-language documents

**3. Stress Tests (10% of test set)**
- Very long inputs (test limits)
- Very short inputs (minimal context)
- Malformed inputs (missing data)

**Example (Meeting Assistant):**
- 4-hour meeting with 30 participants
- 5-minute standup with 3 people
- Meeting with heavy background noise

**4. Adversarial Cases (5% of test set)**
- Deliberately tricky inputs
- Known failure modes
- Attempts to break the system

**Example (Content Moderator):**
- Hate speech disguised as legitimate speech
- Use of code words or symbols
- Sarcasm or context-dependent meaning

**5. Regression Tests (5% of test set)**
- Previously failed cases
- Cases from user-reported bugs
- Fixed issues that shouldn't break again

#### Evaluation Data Sources:

Where to get signals:

**User Feedback:**
- Surveys (CSAT, NPS)
- Ratings (thumbs up/down, 1-5 stars)
- Support tickets (complaints, questions)
- Feature requests (what's missing)

**System Logs:**
- Error rates and types
- Latency/performance metrics
- Usage patterns (frequency, duration)
- User actions (accepts, edits, rejects)

**Business Metrics:**
- Revenue, conversions
- Cost savings
- Time savings
- Retention, churn

**Expert Review:**
- Domain experts evaluate samples
- Spot audits of outputs
- Deep dives on concerning patterns

**Comparative Baselines:**
- Current system performance
- Human baseline (manual process)
- Competitor benchmarks

---

### Step 4: Involve Stakeholders

AI evaluation isn't a solo activity. Different stakeholders provide different perspectives.

#### Who to Involve:

**Domain Experts**

**Role:**
- Validate factual correctness
- Provide ground truth labels
- Define what "good" looks like
- Create evaluation rubrics

**When to Involve:**
- Pre-launch: Define success criteria, create test sets
- During testing: Evaluate sample outputs
- Post-launch: Audit concerning outputs, investigate failures

**How to Engage:**
- Review sessions (show them outputs, get feedback)
- Rubric creation workshops
- Regular audits (monthly or quarterly)

**Example:**
- Medical AI → Doctors review diagnosis suggestions
- Legal AI → Senior lawyers validate contract analysis
- Financial AI → Compliance team checks recommendations

**End Users**

**Role:**
- Evaluate usefulness and user experience
- Provide real-world feedback
- Reveal unexpected use cases

**When to Involve:**
- Pilot testing (before full launch)
- Beta programs (early access)
- Ongoing feedback (surveys, interviews)

**How to Engage:**
- User surveys (CSAT, NPS)
- Interview sessions (qualitative feedback)
- Usage tracking (behavioral data)
- In-app feedback (thumbs up/down)

**Example:**
- Customer support AI → Support agents provide feedback on AI suggestions
- Content assistant → Marketing team rates draft quality
- Code assistant → Developers indicate acceptance/rejection

**Business Stakeholders**

**Role:**
- Define success metrics and priorities
- Allocate resources
- Make go/no-go decisions
- Evaluate ROI

**When to Involve:**
- Upfront: Goal setting, KPI definition
- Regular reviews: Metric dashboards, progress updates
- Decision points: Launch readiness, major changes

**How to Engage:**
- KPI alignment meetings
- Executive dashboards
- Business reviews (monthly/quarterly)
- ROI analysis

**Example:**
- VP of Support → Defines cost savings targets for chatbot
- Head of Sales → Evaluates impact on conversion rates
- CFO → Reviews ROI and budget allocation

**Engineering/Product Teams**

**Role:**
- Implement the AI system
- Iterate based on feedback
- Fix bugs and issues
- Monitor production performance

**When to Involve:**
- Throughout the entire process
- Daily during active development
- On-call for production issues

**How to Engage:**
- Regular syncs on metrics
- Incident reviews (when things break)
- Retrospectives (what worked, what didn't)
- Access to logs and metrics

#### Stakeholder Engagement Plan Template:

```
STAKEHOLDER ENGAGEMENT PLAN:

Pre-Launch:
- [ ] Domain experts: Define "good" examples, create rubrics
- [ ] Business: Align on KPIs and success criteria
- [ ] Users: Participate in pilot, provide early feedback
- [ ] Engineering: Build evaluation infrastructure

During Testing:
- [ ] Domain experts: Rate 50-100 sample outputs/week
- [ ] Users: Provide thumbs up/down, qualitative feedback
- [ ] Business: Review weekly metric reports
- [ ] Engineering: Monitor logs, fix issues

Post-Launch:
- [ ] Domain experts: Monthly audits of concerning outputs
- [ ] Users: Ongoing surveys, support feedback
- [ ] Business: Quarterly business reviews
- [ ] Engineering: Continuous monitoring, incident response
```

#### Feedback Collection Methods:

**Quantitative Feedback:**
- Thumbs up/down on outputs
- 1-5 star ratings
- Likert scale surveys
- NPS/CSAT scores
- Usage metrics (frequency, duration, completion rate)

**Qualitative Feedback:**
- Open-text feedback boxes
- User interviews (30-60 min deep dives)
- Support ticket analysis
- Social media monitoring
- Usability testing sessions

**Best Practices:**
- **Make feedback easy:** One-click ratings, minimal friction
- **Close the loop:** Show users their feedback matters
- **Act on feedback:** Visible improvements based on input
- **Regular cadence:** Weekly or monthly, not ad-hoc

---

### Step 5: Plan Iteration

No AI system is perfect on day one. Plan for continuous improvement.

#### When Things Don't Work:

**Diagnosis Process:**

**1. Identify: What's not working?**
- Which metrics are below target?
- What user feedback indicates problems?
- Are there patterns in failures?

**Sources:**
- Automated metric dashboards
- User complaints/support tickets
- Domain expert reviews
- Error logs

**2. Diagnose: Why is it failing?**

Common root causes:

**Prompt/Instruction Issues:**
- Instructions unclear or ambiguous
- Missing important context
- Conflicting requirements

**Data Quality:**
- Training data doesn't match production
- Missing edge cases in training
- Biased or unrepresentative data

**Model Limitations:**
- Task too complex for model capability
- Model hallucinating or confabulating
- Context window too small

**Integration Issues:**
- Incorrect input processing
- Poor error handling
- UX problems (users don't understand)

**3. Hypothesize: What might fix it?**

Potential solutions:

**For Prompt Issues:**
- Rewrite prompts with clearer instructions
- Add few-shot examples
- Break complex tasks into subtasks
- Add explicit constraints or requirements

**For Data Issues:**
- Collect more representative data
- Add examples of failure cases
- Balance dataset across categories
- Improve data quality

**For Model Issues:**
- Try a more capable model
- Add retrieval/search for grounding
- Implement fact-checking layer
- Add human review for certain cases

**For Integration Issues:**
- Improve input validation
- Better error messages
- Add guardrails (input/output filters)
- Improve UX (clearer expectations)

**4. Test: Try the fix on a representative sample**

Before deploying:
- Test on 50-100 examples
- Include cases that were failing
- Ensure fix doesn't break other things
- Get stakeholder review

**5. Validate: Does it improve metrics?**

Check:
- Did target metric improve?
- Did other metrics stay the same or improve?
- Is the fix sustainable (not just overfitting)?

**6. Deploy: Roll out if validated**

Deployment strategy:
- Shadow mode first (monitor, don't act)
- Phased rollout (10% → 50% → 100%)
- Monitor closely (daily checks)
- Have rollback plan

**7. Monitor: Watch for unintended consequences**

Post-deployment monitoring:
- Did metrics actually improve in production?
- Any unexpected side effects?
- User feedback positive?
- Business impact as expected?

#### Improvement Cycle Template:

```
IMPROVEMENT CYCLE:

1. IDENTIFY
   What: [Metric below target, user complaint pattern]
   Evidence: [Specific data points, examples]

2. DIAGNOSE
   Root Cause: [Why it's failing]
   Analysis: [Error analysis, pattern identification]

3. HYPOTHESIZE
   Proposed Fix: [What we'll try]
   Expected Impact: [How metrics should improve]

4. TEST
   Sample Size: [N examples tested]
   Results: [Did it work on test set?]

5. VALIDATE
   Metrics: [Before vs after on holdout set]
   Stakeholder Review: [Expert/user feedback]

6. DEPLOY
   Rollout Plan: [Phased approach]
   Monitoring: [What to watch]

7. MONITOR
   Post-Deployment Metrics: [Actual improvement]
   Unintended Consequences: [Any surprises]
```

#### Scaling Evaluation:

As your AI system matures:

**Start Small:**
- 50-100 test cases initially
- Focus on high-impact, high-frequency scenarios
- Manual evaluation to understand quality

**Build Automation:**
- Convert manual checks to automated where possible
- Create regression test suites
- Set up continuous monitoring

**Expand Coverage:**
- Add more test cases as you find edge cases
- Include user-reported failures
- Test new features/capabilities

**Maintain Quality:**
- Regular audits (monthly/quarterly)
- Update test sets as product evolves
- Refresh evaluation criteria as goals change

**Best Practices:**
- Evaluation is ongoing, not one-time
- Budget time for evaluation (10-20% of development)
- Celebrate improvements (show team impact)
- Learn from failures (blameless postmortems)

---

## Common AI System Types & Evaluation Approaches

Different AI systems require different evaluation strategies. Here are common patterns:

### 1. Customer Support Chatbot

**System Description:**
Handles customer inquiries via chat, email, or messaging platforms.

**Success KPIs:**
- **Resolution rate:** % of issues solved without human intervention
- **User satisfaction:** CSAT score from customers
- **Response time:** Speed of initial and complete responses
- **Deflection rate:** % of inquiries not escalated to human agents
- **Cost savings:** Reduction in support headcount or hours

**Evaluation Strategy:**

**Automated Metrics:**
- Response time (from logs)
- Escalation rate
- Conversation length
- Resolution keywords ("solved", "thank you")

**Human Evaluation:**
- Support team reviews AI responses (quality, accuracy)
- Sample 50-100 conversations/week
- Rate on helpfulness, correctness, tone

**Real-World Validation:**
- A/B test: AI chatbot vs traditional support
- Measure CSAT, resolution rate, cost per ticket
- Monitor customer complaints about AI

**Test Set:**
- Common questions (password reset, order status)
- Complex issues (refunds, technical problems)
- Edge cases (angry customers, multiple issues)
- Off-topic/adversarial (trying to break bot)

**Key Risks:**
- Giving wrong information (factual errors)
- Frustrating customers (not understanding, repetitive)
- Missing urgent issues (security, account compromise)
- Escalating too often (defeats purpose)

**Iteration Focus:**
- Improve knowledge base coverage
- Better urgency detection
- More empathetic responses
- Faster escalation paths

---

### 2. Content Generation (Marketing Copy, Emails, etc.)

**System Description:**
Generates marketing content, emails, product descriptions, or other written materials.

**Success KPIs:**
- **Acceptance rate:** % of AI content used as-is or with minor edits
- **Time savings:** vs manual content creation
- **Content performance:** Click rates, conversions, engagement
- **Brand consistency:** Adherence to brand voice and guidelines
- **Volume:** Content produced per week/month

**Evaluation Strategy:**

**Automated Metrics:**
- Brand keyword usage
- Tone/sentiment analysis
- Length/format compliance
- Grammar/spelling checks

**Human Evaluation:**
- Marketing team rates on-brand, quality, creativity
- Use rubric: Brand fit, quality, originality (1-5 scale)
- Sample 20-30 pieces/week

**Real-World Validation:**
- A/B test: AI content vs human content performance
- Measure clicks, conversions, engagement
- Monitor customer feedback/comments

**Test Set:**
- Various content types (emails, social, blog posts)
- Different tones (professional, casual, urgent)
- Product categories (if e-commerce)
- Campaign types (promotion, education, retention)

**Key Risks:**
- Off-brand content (wrong tone, style)
- Factual errors (wrong product details, prices)
- Offensive or inappropriate language
- Generic/uninspired content

**Iteration Focus:**
- Better brand voice prompts
- More specific product information
- A/B testing variants
- Fine-tuning on best-performing content

---

### 3. Document Analysis/Extraction

**System Description:**
Extracts structured information from unstructured documents (invoices, contracts, forms).

**Success KPIs:**
- **Extraction accuracy:** % of fields extracted correctly
- **Processing time:** vs manual data entry
- **Error rate:** % of documents with mistakes
- **Coverage:** % of document types handled
- **Cost savings:** Reduction in manual labor

**Evaluation Strategy:**

**Automated Metrics:**
- Field-level accuracy (exact match to ground truth)
- Confidence scores
- Processing latency
- Extraction completeness (all fields present)

**Human Evaluation:**
- Domain experts verify extractions
- Sample 50-100 documents/week
- Focus on high-value or high-risk extractions

**Real-World Validation:**
- Shadow mode: AI extracts in parallel with humans
- Compare accuracy and speed
- Monitor downstream impact (bad data → bad decisions)

**Test Set:**
- Various document types (invoices, contracts, forms)
- Quality levels (pristine PDFs, scanned, handwritten)
- Edge cases (multi-page, unusual formats)
- Different vendors/templates

**Key Risks:**
- Missed critical information (legal clauses, payment terms)
- Incorrect extractions leading to bad decisions
- Handling novel document formats
- Low confidence on borderline cases

**Iteration Focus:**
- Improve OCR quality
- Better template matching
- Human review for low-confidence cases
- Expand to new document types

---

### 4. Code Assistant/Generation

**System Description:**
Helps developers write code (suggestions, completions, generation, debugging).

**Success KPIs:**
- **Acceptance rate:** % of suggestions used by developers
- **Developer productivity:** Tasks completed per day
- **Code quality:** Bugs, security issues, review comments
- **Time to completion:** Faster development cycles
- **Developer satisfaction:** NPS from engineering team

**Evaluation Strategy:**

**Automated Metrics:**
- Acceptance rate (from IDE logs)
- Code correctness (passes tests)
- Security scan results
- Code complexity metrics

**Human Evaluation:**
- Engineers review AI-generated code
- Rate on correctness, style, efficiency
- Code review feedback on AI contributions

**Real-World Validation:**
- A/B test: developers with vs without AI
- Measure productivity, code quality, satisfaction
- Monitor bug rates in AI-assisted code

**Test Set:**
- Various coding tasks (functions, classes, algorithms)
- Multiple languages (Python, JavaScript, Java, etc.)
- Edge cases (complex logic, edge cases, error handling)
- Security-sensitive code

**Key Risks:**
- Insecure code (SQL injection, XSS vulnerabilities)
- Incorrect logic (subtle bugs, off-by-one errors)
- Technical debt (unmaintainable code)
- Copying copyrighted code

**Iteration Focus:**
- Better context understanding
- Security-first code generation
- Style consistency
- More thorough testing suggestions

---

### 5. Medical Diagnosis Aid

**System Description:**
Helps doctors interpret symptoms, suggest possible diagnoses, recommend tests.

**Success KPIs:**
- **Diagnostic accuracy:** Agreement with final diagnosis
- **Doctor efficiency:** Time saved per patient
- **Patient outcomes:** Correct treatment, faster recovery
- **Safety:** Zero missed critical conditions
- **Doctor satisfaction:** Trust and usability

**Evaluation Strategy:**

**Automated Metrics:**
- Symptom coverage (all symptoms considered)
- Evidence-based recommendations (citations)
- Consistency with guidelines

**Human Evaluation:**
- Doctors review AI suggestions on real cases
- Rate diagnostic quality, helpfulness
- Monthly audits by medical board

**Real-World Validation:**
- Shadow mode first (AI suggests, doctors decide)
- Track diagnostic accuracy vs doctor-only
- Monitor patient outcomes
- Regulatory approval process

**Test Set:**
- Common conditions (flu, diabetes, hypertension)
- Rare conditions (edge cases)
- Emergency cases (heart attack, stroke)
- Ambiguous cases (multiple possible diagnoses)

**Key Risks:**
- Under-triaging life-threatening conditions (HIGHEST RISK)
- Missing rare but serious conditions
- Over-triaging (unnecessary ER visits, anxiety)
- Liability if AI gives wrong advice

**Iteration Focus:**
- Extreme emphasis on recall for serious conditions
- Clear disclaimers (AI doesn't replace doctors)
- Extensive testing on diverse patient populations
- Regular updates with latest medical research

---

## Practical Templates & Checklists

### Pre-Launch Evaluation Checklist

**System Understanding:**
- [ ] Documented system function and purpose
- [ ] Identified all user types
- [ ] Defined business value and ROI
- [ ] Assessed risk level
- [ ] Mapped inputs, outputs, and workflow

**Success Criteria:**
- [ ] Defined primary business KPI
- [ ] Set quality bars (with specific numbers)
- [ ] Identified deal-breakers
- [ ] Aligned with business stakeholders
- [ ] Documented user experience goals

**Evaluation Strategy:**
- [ ] Created test set (100+ examples)
- [ ] Defined automated metrics
- [ ] Set up human evaluation process
- [ ] Planned A/B test or pilot
- [ ] Built evaluation infrastructure (logging, dashboards)

**Stakeholder Involvement:**
- [ ] Engaged domain experts for rubrics
- [ ] Recruited users for pilot
- [ ] Aligned with business on metrics
- [ ] Briefed engineering on monitoring

**Iteration Plan:**
- [ ] Set up metric monitoring
- [ ] Defined alert thresholds
- [ ] Created incident response process
- [ ] Planned feedback collection
- [ ] Scheduled regular reviews

**Launch Readiness:**
- [ ] Met quality bar on test set
- [ ] Positive feedback from pilot users
- [ ] Business stakeholders approved
- [ ] Monitoring infrastructure in place
- [ ] Rollback plan ready

---

### Ongoing Monitoring Template

**Daily:**
- [ ] Check automated metric dashboard
- [ ] Review error logs
- [ ] Monitor user feedback
- [ ] Respond to critical incidents

**Weekly:**
- [ ] Review key metrics vs targets
- [ ] Analyze user feedback patterns
- [ ] Domain expert review of 50 samples
- [ ] Engineering sync on issues

**Monthly:**
- [ ] Deep dive error analysis
- [ ] User satisfaction survey
- [ ] Business metric review
- [ ] Update test set with new failure cases

**Quarterly:**
- [ ] Comprehensive system evaluation
- [ ] Stakeholder business review
- [ ] Model retraining/updating
- [ ] Roadmap planning for improvements

---

### Incident Response Template

When things go wrong:

```
INCIDENT RESPONSE:

1. DETECT (Within minutes)
   - Automated alert fires
   - User complaint received
   - Metric drops below threshold

2. ASSESS (Within 1 hour)
   - Severity: [Critical/High/Medium/Low]
   - Impact: [# users, business impact]
   - Root Cause: [Hypothesis]

3. MITIGATE (Within 2-4 hours)
   - Immediate action: [Rollback, disable feature, add guardrails]
   - User communication: [What to tell users]
   - Monitoring: [What to watch]

4. RESOLVE (Within 1-3 days)
   - Fix: [Permanent solution]
   - Test: [Validation]
   - Deploy: [Rollout plan]

5. LEARN (Within 1 week)
   - Postmortem: [What happened, why, how to prevent]
   - Update process: [Changes to avoid repeat]
   - Document: [Add to runbook]
```

---

## Key Principles

Throughout all evaluation efforts, keep these principles in mind:

### 1. Business-First Thinking

**Always connect back to business value:**
- Metrics must matter to the business, not just be technically interesting
- ROI is ultimate success measure
- User value > algorithmic performance

**Example:**
- ❌ "Our model has 95% accuracy" (so what?)
- ✅ "We're reducing support costs by 40% while maintaining 78% CSAT" (business impact)

### 2. Pragmatic Evaluation

**Balance thoroughness with speed:**
- Perfect is the enemy of good
- Start simple, add complexity as needed
- 80/20 rule: focus on high-impact areas
- Evaluate what matters, not everything

**Example:**
- Don't spend weeks building perfect test infrastructure
- Start with 50 manual evaluations
- Automate the high-value checks
- Iterate based on what you learn

### 3. User-Centric

**End user experience matters most:**
- High accuracy means nothing if users don't adopt it
- Involve real users early and often
- Don't optimize metrics that don't reflect user value

**Example:**
- A chatbot with 99% accuracy that users hate is a failure
- A 85% accurate bot that users love and trust is a success

### 4. Risk-Aware

**Understand what can go wrong:**
- Higher stakes = more rigorous evaluation
- Plan for failure modes
- Have mitigation strategies ready
- Different systems have different acceptable risk levels

**Example:**
- Medical AI: Zero tolerance for missing serious conditions
- Email categorizer: Some mistakes are acceptable
- Financial AI: Regulatory compliance is non-negotiable

### 5. Iterative Mindset

**Evaluation is ongoing, not one-time:**
- Build feedback loops
- Continuous improvement is the goal
- Learn from production, not just test sets
- Plan to be wrong and iterate

**Example:**
- Launch with 80% quality if low-risk
- Collect real user feedback
- Iterate weekly based on what you learn
- Reach 95% quality through iteration

---

## Common Pitfalls to Avoid

**❌ Evaluating only on test sets:**
- Test sets don't capture real-world complexity
- Distribution shifts in production
- Missing unknown unknowns

**✅ Instead:** Combine test sets, pilots, A/B tests, and continuous monitoring

---

**❌ Optimizing the wrong metrics:**
- High accuracy on irrelevant tasks
- Technical metrics that don't translate to business value

**✅ Instead:** Start with business KPIs, work backwards to technical metrics

---

**❌ Ignoring stakeholders:**
- Building in a vacuum
- Missing domain expert knowledge
- Not getting user buy-in

**✅ Instead:** Involve stakeholders from day one, collect diverse perspectives

---

**❌ Over-engineering evaluation:**
- Perfect evaluation infrastructure that takes months
- Analyzing everything when 80/20 would suffice

**✅ Instead:** Start simple, build only what you need, iterate

---

**❌ One-time evaluation:**
- Evaluate once before launch, never again
- Assuming quality stays constant

**✅ Instead:** Continuous evaluation, regular reviews, adapt to changes

---

**❌ Assuming higher accuracy is always better:**
- Chasing 99% when 90% is good enough
- Missing business trade-offs (cost, latency, UX)

**✅ Instead:** Define "good enough" based on business needs and constraints

---

## Adapting the Framework

This framework is flexible. Adapt it to your context:

**For Early-Stage/MVP:**
- Focus on Step 1 (understanding) and Step 2 (success criteria)
- Lightweight evaluation (manual reviews)
- Rapid iteration based on qualitative feedback

**For Mature Systems:**
- Robust automated evaluation
- Extensive test coverage
- Sophisticated monitoring and alerting

**For High-Stakes Systems:**
- Rigorous human evaluation
- External audits
- Extensive safety testing
- Regulatory approval processes

**For Low-Stakes Systems:**
- Lighter evaluation
- Focus on user satisfaction
- Faster iteration cycles

---

## Conclusion

Evaluating AI systems is as much art as science. The key is to be:

- **Systematic:** Follow a structured approach
- **Business-focused:** Connect to real value
- **User-centric:** Prioritize user experience
- **Pragmatic:** Balance rigor with speed
- **Iterative:** Continuously improve

Use this framework as a starting point, adapt it to your needs, and remember: evaluation is not a checkpoint, it's a continuous process.

---

**Additional Resources:**

- For interview preparation, see: `EVALS_PRACTICE_SCENARIOS.md`
- For quick reference, see: `wiki/` directory
- For thought leadership, see: `BLOG_AI_EVALUATION_FRAMEWORK.md`

---

*Last updated: December 15, 2025*
