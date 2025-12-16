# Implementation Guide

Step-by-step guide to building compound AI systems.

## The Five-Step Process

Building a compound AI system follows a structured approach:

1. Decompose the problem
2. Design agent responsibilities
3. Build orchestration logic
4. Implement quality gates
5. Build feedback loops

---

## Step 1: Decompose the Problem

Break down your use case into the [five components](Five-Component-Framework.md).

### Example: Email Auto-Responder

**1. Sensing:** Parse incoming emails, extract intent, identify urgency

**2. Knowledge:** Email history, company policies, response templates

**3. Thinking:** Classify email type, generate appropriate response, check tone

**4. Actions:** Send reply, schedule follow-up, escalate if needed

**5. Evaluation:** Track response quality, collect user feedback, monitor escalation rate

### Exercise

Write down your use case and map it to the five components:
- What inputs do you process?
- What knowledge do you need?
- What decisions must be made?
- What actions does the system take?
- How do you know if it's working?

---

## Step 2: Design Agent Responsibilities

For each component requiring AI, define specialized agents.

### Principles

**Single Responsibility**
Each agent has one clear job

**Clear Inputs/Outputs**
Structured data contracts between agents

**Testable in Isolation**
Can validate each agent independently

**Composable**
Agents can be recombined for different workflows

### Agent Design Template

```
AGENT: [Name]
RESPONSIBILITY: [One sentence]
INPUT: [Structured data format]
OUTPUT: [Structured data format]
SUCCESS CRITERIA: [How to evaluate this agent]
```

### Example: Planning Assistant

| Agent | Responsibility | Input | Output |
|-------|---------------|-------|--------|
| **Parser** | Extract task details | Natural language text | Task object (name, duration, deadline) |
| **Analyzer** | Detect scheduling conflicts | Task + Calendar | Feasibility report with conflicts |
| **Planner** | Generate optimal schedule | Task + Constraints | Proposed schedule |
| **Presenter** | Show options to user | Schedule alternatives | User selection |
| **Executor** | Apply approved changes | Approved plan | Updated calendar |

---

## Step 3: Build Orchestration Logic

Define how agents coordinate.

### Basic Orchestration Pattern

```python
def orchestrate_workflow(input_data):
    # Stage 1: Parse and validate
    parsed = parser_agent.parse(input_data)
    if not parsed.valid:
        return request_more_info(parsed.missing)

    # Stage 2: Get context
    context = knowledge_base.get_context(parsed)

    # Stage 3: Analyze
    analysis = analyzer_agent.analyze(parsed, context)

    # Stage 4: Generate plan
    if analysis.has_issues:
        options = planner_agent.generate_alternatives(parsed, context)
        plan = get_user_choice(options)
    else:
        plan = planner_agent.generate_plan(parsed, context)

    # Stage 5: Validate
    if not validator.validate(plan):
        return validation_error

    # Stage 6: Execute (with approval)
    if user_approves(plan):
        result = executor_agent.execute(plan)
        return result

    return None
```

### Error Handling

```python
def safe_orchestration(input_data):
    try:
        result = orchestrate_workflow(input_data)
        return result

    except ParsingError as e:
        logger.error(f"Parsing failed: {e}")
        return request_clarification()

    except ValidationError as e:
        logger.error(f"Validation failed: {e}")
        return explain_validation_failure(e)

    except ExecutionError as e:
        logger.error(f"Execution failed: {e}")
        rollback_changes()
        return execution_failure_message(e)
```

---

## Step 4: Implement Quality Gates

Add validation at critical points.

### Input Validation

```python
def validate_input(task):
    """Check input completeness and validity"""

    # Required fields
    required = ['name', 'duration', 'deadline']
    if not all(field in task for field in required):
        return False, f"Missing: {', '.join(required)}"

    # Logical constraints
    if task.deadline < datetime.now():
        return False, "Deadline is in the past"

    if task.duration <= 0:
        return False, "Duration must be positive"

    return True, None
```

### Output Validation

```python
def validate_schedule(schedule, constraints):
    """Check schedule meets all constraints"""

    for event in schedule:
        # Time bounds check
        if not within_hours(event, start=8, end=22):
            return False, "Event outside working hours"

        # Conflict check
        if has_overlap(event, existing_events):
            return False, "Schedule conflict detected"

        # Constraint check
        for constraint in constraints:
            if constraint.violated_by(event):
                return False, f"Violates {constraint.name}"

    return True, None
```

### Quality Scoring

```python
def score_quality(output):
    """Score output quality on 0-1 scale"""

    scores = []

    # Completeness
    completeness = len(output.required_fields_present) / len(output.required_fields)
    scores.append(completeness)

    # Coherence (using LLM)
    coherence = llm_evaluator.rate_coherence(output.text)
    scores.append(coherence)

    # Format compliance
    format_score = 1.0 if output.matches_schema() else 0.0
    scores.append(format_score)

    return sum(scores) / len(scores)
```

---

## Step 5: Build Feedback Loops

Enable continuous improvement.

### Track Performance

```python
class PerformanceTracker:
    def log_execution(self, input, output, success):
        """Log each execution for analysis"""
        self.execution_log.append({
            'timestamp': datetime.now(),
            'input': input,
            'output': output,
            'success': success,
            'duration': self.timer.elapsed()
        })

    def get_success_rate(self, last_n_days=7):
        """Calculate success rate"""
        recent = [e for e in self.execution_log
                  if e['timestamp'] > datetime.now() - timedelta(days=last_n_days)]

        if not recent:
            return None

        successes = sum(1 for e in recent if e['success'])
        return successes / len(recent)
```

### Collect User Feedback

```python
def collect_feedback(execution_id, rating, comments=None):
    """Store user feedback"""
    feedback_db.save({
        'execution_id': execution_id,
        'rating': rating,  # 1-5 stars
        'comments': comments,
        'timestamp': datetime.now()
    })

def analyze_feedback():
    """Identify improvement opportunities"""
    low_rated = feedback_db.query(rating__lt=3)

    patterns = {}
    for feedback in low_rated:
        # Find common patterns in failures
        failure_type = classify_failure(feedback)
        patterns[failure_type] = patterns.get(failure_type, 0) + 1

    return sorted(patterns.items(), key=lambda x: x[1], reverse=True)
```

### Learn and Adapt

```python
class AdaptiveSystem:
    def learn_from_data(self):
        """Update system based on historical data"""

        # Analyze estimation errors
        errors = self.analyze_estimation_errors()

        if errors['avg_underestimate'] > 15:
            self.buffer_percentage += 0.05
            logger.info("Increased buffer to", self.buffer_percentage)

        # Identify frequently failing constraints
        violations = self.analyze_constraint_violations()

        for constraint, count in violations.items():
            if count > 10:
                logger.warn(f"Constraint '{constraint}' frequently violated")
                # Could adjust or remove constraint
```

---

## Implementation Checklist

### ✅ Planning Phase

- [ ] Decomposed problem into 5 components
- [ ] Defined agent responsibilities
- [ ] Created agent interface contracts
- [ ] Designed orchestration flow
- [ ] Identified quality gates needed

### ✅ Building Phase

- [ ] Implemented individual agents
- [ ] Built orchestration logic
- [ ] Added input validation
- [ ] Added output validation
- [ ] Implemented error handling

### ✅ Testing Phase

- [ ] Unit tested each agent in isolation
- [ ] Integration tested full workflow
- [ ] Tested edge cases and failures
- [ ] Validated with real users

### ✅ Production Phase

- [ ] Set up monitoring
- [ ] Implemented feedback collection
- [ ] Built performance dashboard
- [ ] Documented system architecture

---

## Common Implementation Mistakes

### ❌ Mistake 1: Skipping Decomposition

**Problem:** Jump straight to coding without planning

**Solution:** Always start with the 5-component framework

---

### ❌ Mistake 2: Vague Agent Responsibilities

**Problem:** Agent that "does everything" is hard to debug

**Solution:** Single responsibility per agent, clear contracts

---

### ❌ Mistake 3: No Validation

**Problem:** Errors propagate through the system

**Solution:** Validate at input, between stages, and at output

---

### ❌ Mistake 4: Poor Error Handling

**Problem:** System crashes on unexpected inputs

**Solution:** Try-catch blocks, graceful degradation, user-friendly errors

---

### ❌ Mistake 5: No Feedback Loop

**Problem:** Can't improve over time

**Solution:** Log executions, collect feedback, analyze patterns

---

## Next Steps

**See a complete example:**
- [Case Study: Planning Assistant →](Case-Study-Planning-Assistant.md)

**Learn best practices:**
- [Best Practices →](Best-Practices-Compound-AI.md)

---

[← Multi-Agent Patterns](Multi-Agent-Patterns.md) | [Next: Case Study →](Case-Study-Planning-Assistant.md)
