# Templates & Checklists

Ready-to-use tools for AI system evaluation.

## System Profile Template

```
SYSTEM: [Name]
DATE: [Date]

FUNCTION:
[One sentence: what it does]

USERS:
[Who uses it, how often, what for]

BUSINESS VALUE:
[Why it matters, expected impact, ROI]

RISK LEVEL: [High/Medium/Low]
JUSTIFICATION: [Why this risk level]

INPUT: [What goes in]
OUTPUT: [What comes out]

WORKFLOW INTEGRATION:
[How it fits into existing processes]

SUCCESS DEPENDENCY:
[What needs to be true for this to work]
```

---

## Success Definition Template

```
SYSTEM: [Name]

PRIMARY KPI: [#1 business metric]
TARGET: [Specific number + timeframe]

QUALITY BARS:
- [Metric 1]: [Threshold]
- [Metric 2]: [Threshold]
- [Metric 3]: [Threshold]

USER EXPERIENCE:
- [Metric]: [Threshold]
- [Metric]: [Threshold]

DEAL-BREAKERS:
- [What makes this unacceptable]
- [Critical failure modes]

NICE-TO-HAVES:
- [Secondary goals]
- [Future improvements]
```

---

## Pre-Launch Checklist

### System Understanding
- [ ] Documented system function
- [ ] Identified all user types
- [ ] Defined business value and ROI
- [ ] Assessed risk level
- [ ] Mapped inputs, outputs, workflow

### Success Criteria
- [ ] Defined primary business KPI
- [ ] Set quality bars (with numbers)
- [ ] Identified deal-breakers
- [ ] Aligned with business stakeholders
- [ ] Documented UX goals

### Evaluation Strategy
- [ ] Created test set (100+ examples)
- [ ] Defined automated metrics
- [ ] Set up human evaluation process
- [ ] Planned A/B test or pilot
- [ ] Built evaluation infrastructure

### Stakeholder Involvement
- [ ] Engaged domain experts for rubrics
- [ ] Recruited users for pilot
- [ ] Aligned with business on metrics
- [ ] Briefed engineering on monitoring

### Iteration Plan
- [ ] Set up metric monitoring
- [ ] Defined alert thresholds
- [ ] Created incident response process
- [ ] Planned feedback collection
- [ ] Scheduled regular reviews

### Launch Readiness
- [ ] Met quality bar on test set
- [ ] Positive feedback from pilot
- [ ] Business stakeholder approval
- [ ] Monitoring infrastructure ready
- [ ] Rollback plan in place

---

## Evaluation Plan Template

```
SYSTEM: [Name]
EVALUATION DATE: [Date]

TIER 1: AUTOMATED METRICS
Metrics:
- [Metric]: [Target]
- [Metric]: [Target]
Data Source: [Where measured]
Frequency: [How often checked]

TIER 2: HUMAN EVALUATION
Who Evaluates: [Domain experts, users, QA]
Sample Size: [N per week/month]
Method: [Rubric, comparative, binary]
Criteria:
- [Criterion 1]: [Scale]
- [Criterion 2]: [Scale]

TIER 3: REAL-WORLD VALIDATION
Approach: [A/B test, shadow mode, pilot]
Duration: [Timeframe]
Success Criteria: [What indicates success]
Metrics to Track:
- [Business metric]
- [User behavior]
- [Feedback]

TEST SET:
- Happy path: [N examples]
- Edge cases: [N examples]
- Stress tests: [N examples]
- Adversarial: [N examples]
- Regression: [N examples]

TOTAL: [Total test cases]
```

---

## Stakeholder Engagement Plan

```
SYSTEM: [Name]

PRE-LAUNCH:
Domain Experts:
- [ ] Define "good" examples
- [ ] Create evaluation rubrics
- [ ] Review [N] sample outputs

Business:
- [ ] Align on KPIs
- [ ] Set success thresholds
- [ ] Approve budget and timeline

Users:
- [ ] Participate in pilot ([N] users)
- [ ] Provide early feedback
- [ ] Test key workflows

Engineering:
- [ ] Build evaluation infrastructure
- [ ] Set up logging and monitoring
- [ ] Create dashboards

DURING TESTING:
Domain Experts: Review [N] samples/week
Users: Provide thumbs up/down + feedback
Business: Review weekly metric reports
Engineering: Monitor logs, fix issues

POST-LAUNCH:
Domain Experts: Monthly audits
Users: Ongoing surveys, feedback
Business: Quarterly business reviews
Engineering: Continuous monitoring, on-call
```

---

## Monitoring Dashboard Template

### Daily Checks
- [ ] Primary KPI vs target
- [ ] Error rate
- [ ] User complaints
- [ ] System latency

### Weekly Review
- [ ] All metrics vs targets
- [ ] User feedback summary
- [ ] Domain expert reviews
- [ ] Engineering issues

### Monthly Analysis
- [ ] Trend analysis
- [ ] Deep error dive
- [ ] User satisfaction survey
- [ ] Business metric review

### Quarterly Planning
- [ ] Comprehensive evaluation
- [ ] Stakeholder business review
- [ ] Model updates
- [ ] Roadmap planning

---

## Incident Response Template

```
INCIDENT: [Title]
DATE: [Date/Time]
SEVERITY: [Critical/High/Medium/Low]

1. DETECT (Minutes)
Alert: [What triggered]
Time: [When detected]

2. ASSESS (1 hour)
Impact: [# users, business impact]
Root Cause Hypothesis: [Initial guess]

3. MITIGATE (2-4 hours)
Immediate Action: [What we did]
User Communication: [What we told users]
Monitoring: [What we're watching]

4. RESOLVE (1-3 days)
Permanent Fix: [Solution]
Validation: [How tested]
Deployment: [Rollout plan]

5. LEARN (1 week)
Root Cause: [Actual cause]
Prevention: [How to avoid]
Process Updates: [Changes made]
Documentation: [Where recorded]
```

---

## Improvement Cycle Template

```
IMPROVEMENT: [Title]
DATE: [Date]

1. IDENTIFY
Problem: [What's not working]
Evidence: [Metrics, feedback, examples]

2. DIAGNOSE
Root Cause: [Why it's failing]
Analysis: [Error patterns, investigation]

3. HYPOTHESIZE
Proposed Fix: [What we'll try]
Expected Impact: [How it should improve]

4. TEST
Sample Size: [N examples]
Results: [Performance on test set]

5. VALIDATE
Metrics Before: [Baseline]
Metrics After: [Improved]
Side Effects: [Any unintended changes]
Stakeholder Review: [Feedback]

6. DEPLOY
Rollout Plan: [Phased approach]
Timeline: [When]
Monitoring: [What to watch]

7. MONITOR
Actual Improvement: [Real results]
Unintended Consequences: [Surprises]
User Feedback: [Reception]
```

---

## Evaluation Rubric Template

```
SYSTEM: [Name]
EVALUATOR: [Name/Role]
DATE: [Date]

SAMPLE ID: [Identifier]

CRITERION 1: [Name]
Score: [1-5]
1 - [Definition]
2 - [Definition]
3 - [Definition]
4 - [Definition]
5 - [Definition]

CRITERION 2: [Name]
Score: [1-5]
[Scale definitions]

CRITERION 3: [Name]
Score: [1-5]
[Scale definitions]

OVERALL ASSESSMENT:
□ Excellent - No issues
□ Good - Minor issues
□ Acceptable - Moderate issues
□ Poor - Major issues
□ Unacceptable - Critical issues

FEEDBACK:
[What's working well]
[What needs improvement]
[Specific issues to address]
```

---

## Next Steps

**Use these templates for your system:**

1. Start with [System Profile Template](#system-profile-template)
2. Define success with [Success Definition Template](#success-definition-template)
3. Plan evaluation with [Evaluation Plan Template](#evaluation-plan-template)
4. Track with [Monitoring Dashboard](#monitoring-dashboard-template)

**Review the framework:**
- [Back to Home](Home.md)
- [Five-Step Framework](1-Five-Step-Framework.md)

---

[← System Types & Examples](6-System-Types-Examples.md) | [Back to Home](Home.md)
