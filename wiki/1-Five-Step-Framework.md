# The 5-Step Evaluation Framework

A systematic approach to evaluating AI systems across their lifecycle.

## Overview

The framework consists of five sequential steps:

1. **Understand the System** - Know what you're evaluating
2. **Define Success** - Make "good" concrete and measurable
3. **Design Evaluation** - Plan how to test systematically
4. **Involve Stakeholders** - Engage the right people at the right time
5. **Plan Iteration** - Build continuous improvement loops

Each step builds on the previous one. Don't skip ahead!

---

## Step 1: Understand the System

**Goal:** Deeply understand what you're evaluating before defining metrics.

**Key Questions:**
- What does this system do?
- Who uses it and why?
- What's the business value?
- What's at stake if it fails?
- How does it fit into existing workflows?

**Output:** System Profile document

**Time Investment:** 1-2 hours

**Why It Matters:** You can't evaluate what you don't understand. This step prevents evaluating the wrong things.

---

## Step 2: Define Success

**Goal:** Make "good" concrete with specific, measurable criteria.

**Key Questions:**
- What business outcome matters most?
- What quality thresholds are acceptable?
- What would make this unacceptable?
- How will users experience success?

**Output:** Success Definition with KPIs and quality bars

**Time Investment:** 2-4 hours (includes stakeholder alignment)

**Why It Matters:** Without clear success criteria, you'll argue about whether it's "good enough" forever.

---

## Step 3: Design Evaluation

**Goal:** Plan systematic testing across multiple dimensions.

**Key Questions:**
- What can be measured automatically?
- What requires human judgment?
- How will you test in the real world?
- What test cases cover your use cases?

**Output:** Evaluation plan with test set, metrics, and validation strategy

**Time Investment:** 1-2 weeks (includes building test infrastructure)

**Why It Matters:** Ad-hoc testing misses edge cases. Systematic evaluation gives confidence.

---

## Step 4: Involve Stakeholders

**Goal:** Engage domain experts, users, and business stakeholders effectively.

**Key Questions:**
- Who can validate correctness?
- Who represents the end user?
- Who makes business decisions?
- How will you collect feedback?

**Output:** Stakeholder engagement plan

**Time Investment:** Ongoing throughout evaluation

**Why It Matters:** No single person has all perspectives. Diverse input prevents blind spots.

---

## Step 5: Plan Iteration

**Goal:** Build systems for continuous improvement when things don't work.

**Key Questions:**
- How will you detect problems?
- How will you diagnose root causes?
- How will you test and deploy fixes?
- How will you prevent regression?

**Output:** Monitoring, incident response, and improvement processes

**Time Investment:** 1-2 weeks (setup) + ongoing

**Why It Matters:** No AI system is perfect on day one. Iteration is how you get to great.

---

## Using the Framework

**For New Systems:**
- Follow all 5 steps in order
- Don't launch without completing Steps 1-3
- Steps 4-5 are crucial for post-launch

**For Existing Systems:**
- Start with Step 1 (you might be surprised what you learn)
- Skip to Step 5 if you already have metrics defined
- Use Step 3 to add rigor to informal testing

**For Quick Assessments:**
- Minimum: Steps 1 and 2 (understand + define success)
- Add Step 3 if you're launching to real users
- Add Steps 4-5 if it's high-stakes

---

## Time Investment

**Minimum (Low-Stakes System):**
- 1-2 days to work through Steps 1-3
- Launch with lightweight monitoring

**Recommended (Most Systems):**
- 2-3 weeks for thorough evaluation
- 10-20% of development time

**High-Stakes System:**
- 1-2 months for rigorous evaluation
- 30-40% of development time
- External audits and reviews

---

## Next Steps

**Ready to dive deeper?** Continue to:

- **[Step 2: Success Metrics →](2-Success-Metrics.md)** - Learn how to define what "good" looks like
- **[Step 3: Evaluation Strategies →](3-Evaluation-Strategies.md)** - Design your testing approach
- **[Templates & Checklists →](7-Templates-Checklists.md)** - Get ready-to-use tools

**Want examples?** See:
- **[System Types & Examples →](6-System-Types-Examples.md)** - Common AI systems and how to evaluate them

---

[← Back to Home](Home.md) | [Next: Success Metrics →](2-Success-Metrics.md)
