# Iteration & Improvement

No AI system is perfect on day one. Here's how to continuously improve.

## The Improvement Cycle

### 1. IDENTIFY
**What's not working?**

**Sources:**
- Metrics below target
- User complaints
- Domain expert reviews
- Error logs

**Look For:**
- Consistent patterns
- Specific categories failing
- Edge cases
- Degradation over time

---

### 2. DIAGNOSE
**Why is it failing?**

**Common Root Causes:**

**Prompt/Instructions:**
- Unclear or ambiguous
- Missing context
- Conflicting requirements

**Data Issues:**
- Distribution shift
- Missing edge cases
- Biased training data

**Model Limitations:**
- Task too complex
- Hallucination
- Context window too small

**Integration:**
- Input processing errors
- Poor error handling
- UX confusion

---

### 3. HYPOTHESIZE
**What might fix it?**

**Potential Solutions:**

**For Prompts:**
- Rewrite with clarity
- Add few-shot examples
- Break into subtasks
- Add explicit constraints

**For Data:**
- Collect more examples
- Balance dataset
- Add failure cases
- Improve quality

**For Models:**
- Use more capable model
- Add retrieval/grounding
- Implement fact-checking
- Add human review layer

**For Integration:**
- Improve validation
- Better error messages
- Add guardrails
- Enhance UX

---

### 4. TEST
**Try the fix on a sample**

- Test on 50-100 examples
- Include failing cases
- Ensure no regressions
- Get stakeholder review

---

### 5. VALIDATE
**Does it improve metrics?**

**Check:**
- Target metric improved?
- Other metrics stable?
- Sustainable (not overfitting)?
- User feedback positive?

---

### 6. DEPLOY
**Roll out if validated**

**Strategy:**
- Shadow mode first
- Phased rollout (10% → 50% → 100%)
- Monitor closely (daily checks)
- Have rollback plan ready

---

### 7. MONITOR
**Watch for unintended consequences**

**Post-Deployment:**
- Metrics tracking
- User feedback
- Error monitoring
- Business impact

---

## Monitoring Strategy

### Daily
- Check dashboards
- Review error logs
- Monitor user feedback
- Respond to incidents

### Weekly
- Metric review vs targets
- User feedback analysis
- Domain expert review (50 samples)
- Engineering sync

### Monthly
- Deep error analysis
- User satisfaction survey
- Business metric review
- Update test sets

### Quarterly
- Comprehensive evaluation
- Stakeholder business review
- Model retraining/updating
- Roadmap planning

---

## Incident Response

When things go wrong:

### DETECT (Minutes)
- Automated alert
- User complaint
- Metric drop

### ASSESS (1 hour)
- Severity: Critical/High/Medium/Low
- Impact: # users, business impact
- Root cause hypothesis

### MITIGATE (2-4 hours)
- Immediate action: Rollback, disable, add guardrails
- User communication
- Monitoring plan

### RESOLVE (1-3 days)
- Permanent fix
- Validation
- Deployment

### LEARN (1 week)
- Postmortem
- Process updates
- Documentation

---

## Scaling Evaluation

### Start Small
- 50-100 test cases
- Focus on high-impact scenarios
- Manual evaluation

### Build Automation
- Convert manual → automated
- Create regression suites
- Set up continuous monitoring

### Expand Coverage
- Add test cases from failures
- Include user-reported issues
- Test new features

### Maintain Quality
- Regular audits
- Update test sets
- Refresh criteria as product evolves

---

## Best Practices

**Budget for evaluation:**
- 10-20% of development time
- Continuous, not one-time
- Part of process, not afterthought

**Celebrate improvements:**
- Show team impact
- Share wins
- Recognize contributors

**Learn from failures:**
- Blameless postmortems
- Document lessons
- Update processes

**Stay agile:**
- Quick iteration cycles
- Don't wait for perfection
- Validate with real users

---

## Common Pitfalls

❌ **One-and-done evaluation**
- Evaluate once, never again
- Quality degrades over time

✅ **Continuous evaluation and monitoring**

---

❌ **No clear ownership**
- Who monitors?
- Who responds to incidents?

✅ **Assign evaluation owners, on-call rotation**

---

❌ **Slow iteration**
- Weeks to deploy fixes
- Missing learning opportunities

✅ **Quick cycles, automated deployment**

---

## Next Steps

**See examples:**
- [System Types & Examples →](6-System-Types-Examples.md)

**Use templates:**
- [Templates & Checklists →](7-Templates-Checklists.md)

---

[← Stakeholder Involvement](4-Stakeholder-Involvement.md) | [Next: System Types & Examples →](6-System-Types-Examples.md)
