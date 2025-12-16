# Best Practices for Compound AI Systems

Patterns to follow and pitfalls to avoid when building multi-agent AI systems.

## Design Principles

### 1. Design for Debuggability

Make it easy to understand what went wrong.

**Log agent decisions:**
```python
logger.info(f"[Parser] Extracted: {task.name}, duration={task.duration}")
logger.info(f"[Analyzer] Found {len(conflicts)} conflicts")
logger.info(f"[Planner] Generated {len(options)} alternatives")
logger.info(f"[Validator] Schedule validation: {result}")
```

**Use structured outputs:**
```python
@dataclass
class AgentOutput:
    success: bool
    result: Any
    reasoning: str
    confidence: float
    processing_time: float
```

**Enable step-through mode:**
```python
def orchestrate(input_data, debug=False):
    result = parser.parse(input_data)

    if debug:
        print(f"Step 1 - Parser: {result}")
        input("Press Enter to continue...")

    # Continue with next steps...
```

---

### 2. Make Decisions Transparent

Users should understand what the system did and why.

**Explain reasoning:**
```python
def explain_schedule(schedule):
    return {
        'schedule': schedule.events,
        'reasoning': [
            "Scheduled during productive hours (2-4 PM)",
            "Added 20% buffer based on similar past tasks",
            "Avoided conflict with soccer practice",
            "Respects no-homework-after-9PM constraint"
        ],
        'alternatives_considered': 3,
        'confidence': 0.85
    }
```

**Show what changed:**
```python
def show_diff(before, after):
    print("Changes to your calendar:")
    for event in after:
        if event not in before:
            print(f"  + Added: {event.name} at {event.start_time}")

    for event in before:
        if event not in after:
            print(f"  - Removed: {event.name}")
```

---

### 3. Plan for Failure Recovery

Systems will fail. Design for graceful degradation.

**Implement fallbacks:**
```python
def generate_plan(task, context):
    try:
        # Try advanced planner
        return advanced_planner.plan(task, context)

    except PlanningError:
        logger.warn("Advanced planner failed, using simple planner")
        return simple_planner.plan(task, context)

    except Exception as e:
        logger.error(f"Both planners failed: {e}")
        return manual_scheduling_required(task)
```

**Retry with exponential backoff:**
```python
def call_llm_with_retry(prompt, max_retries=3):
    for attempt in range(max_retries):
        try:
            return llm.generate(prompt)

        except RateLimitError:
            wait_time = 2 ** attempt
            logger.info(f"Rate limited, waiting {wait_time}s...")
            time.sleep(wait_time)

        except Exception as e:
            if attempt == max_retries - 1:
                raise
            logger.warn(f"Attempt {attempt + 1} failed: {e}")

    raise Exception("LLM call failed after retries")
```

**Rollback on failure:**
```python
def execute_with_rollback(plan):
    original_state = save_state()

    try:
        execute_plan(plan)
        return ExecutionSuccess()

    except Exception as e:
        logger.error(f"Execution failed: {e}")
        restore_state(original_state)
        return ExecutionFailure(e)
```

---

### 4. Optimize for Cost and Latency

**Use smaller models for simple tasks:**
```python
class AgentModelSelector:
    def select_model(self, task_complexity):
        if task_complexity == "simple":
            return "gpt-3.5-turbo"  # $0.001/1K tokens
        elif task_complexity == "complex":
            return "gpt-4"  # $0.03/1K tokens
        else:
            return "claude-3-haiku"  # $0.00025/1K tokens
```

**Cache frequent queries:**
```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def get_calendar_events(date: str):
    """Cache calendar queries"""
    return calendar_api.fetch_events(parse_date(date))
```

**Parallelize independent agents:**
```python
async def parallel_analysis(task, context):
    """Run independent analyses in parallel"""

    results = await asyncio.gather(
        conflict_checker.analyze(task, context),
        feasibility_analyzer.analyze(task, context),
        constraint_validator.analyze(task, context)
    )

    return combine_results(results)
```

---

### 5. Build Incrementally

Don't build everything at once.

**Phase 1: Core workflow**
- Single agent for main functionality
- Basic input/output validation
- Manual testing

**Phase 2: Quality improvements**
- Add specialized agents for weak points
- Implement quality gates
- Add error handling

**Phase 3: Human oversight**
- Add human-in-the-loop for critical decisions
- Build approval interfaces
- Add override mechanisms

**Phase 4: Learning loops**
- Collect usage data
- Analyze patterns
- Build self-improvement

**Phase 5: Optimization**
- Optimize model selection
- Add caching
- Parallelize where possible

---

## Common Pitfalls

### ❌ Pitfall 1: Too Many Agents

**Problem:**
Over-engineering with 15+ agents for a simple task.

**Why it happens:**
- Excitement about multi-agent architectures
- Premature optimization
- Unclear responsibilities

**Solution:**
- Start with 2-3 agents
- Add more only when justified
- Each agent should have clear, distinct purpose

**Example:**
```
❌ Bad: Parser, Validator, Analyzer, Pre-Processor, Post-Processor,
        Formatter, Optimizer, Checker, Reviewer...

✅ Good: Parser, Planner, Executor
```

---

### ❌ Pitfall 2: Unclear Agent Boundaries

**Problem:**
Agents with overlapping responsibilities, unclear ownership.

**Why it happens:**
- Agents evolved organically without design
- Unclear separation of concerns
- Code duplication across agents

**Solution:**
- Use single responsibility principle
- Document each agent's role clearly
- Review agent boundaries regularly

**Example:**
```
❌ Bad:
   Parser: Extract task + validate + check conflicts
   Planner: Check conflicts + generate plan

✅ Good:
   Parser: Extract task only
   Validator: Validate task only
   Conflict Detector: Check conflicts only
   Planner: Generate plan only
```

---

### ❌ Pitfall 3: No Validation Between Stages

**Problem:**
Errors propagate through pipeline undetected.

**Why it happens:**
- Assuming LLMs always return valid outputs
- Trusting upstream components blindly
- No schema enforcement

**Solution:**
- Validate outputs before passing to next stage
- Use structured outputs (JSON schema, Pydantic)
- Add quality gates between agents

**Example:**
```python
❌ Bad:
task = parser.parse(input)
plan = planner.plan(task)  # What if task is invalid?
execute(plan)

✅ Good:
task = parser.parse(input)
if not validator.is_valid(task):
    return request_more_info()

plan = planner.plan(task)
if not validator.is_valid_plan(plan):
    return planning_error()

execute(plan)
```

---

### ❌ Pitfall 4: Ignoring Failure Modes

**Problem:**
System breaks when LLM call fails or returns unexpected format.

**Why it happens:**
- Optimistic coding
- Testing only happy paths
- No error handling strategy

**Solution:**
- Design for failure
- Test error scenarios
- Implement retries and fallbacks

**Example:**
```python
❌ Bad:
response = llm.generate(prompt)
task = json.loads(response)  # What if invalid JSON?

✅ Good:
try:
    response = llm.generate_with_retry(prompt, max_retries=3)
    task = parse_json_safely(response)

    if not task:
        return fallback_parser(prompt)

except LLMError as e:
    logger.error(f"LLM failed: {e}")
    return error_response("AI service temporarily unavailable")
```

---

### ❌ Pitfall 5: Black-Box Decision Making

**Problem:**
Users don't understand why system made certain decisions.

**Why it happens:**
- No logging of reasoning
- Complex agent interactions
- Treating AI as magic box

**Solution:**
- Log reasoning at each step
- Explain decisions to users
- Allow overrides and customization

**Example:**
```python
❌ Bad:
plan = planner.plan(task)
execute(plan)

✅ Good:
plan = planner.plan(task)
explanation = {
    'schedule': plan,
    'why_this_time': planner.reasoning,
    'alternatives': planner.other_options,
    'assumptions': planner.assumptions
}
present_for_approval(explanation)
```

---

### ❌ Pitfall 6: No Feedback Loop

**Problem:**
Can't improve over time, repeat same errors.

**Why it happens:**
- No data collection
- No analysis of failures
- Treating system as "done"

**Solution:**
- Log all executions
- Collect user feedback
- Analyze patterns regularly
- Iterate based on data

**Example:**
```python
✅ Good:
class FeedbackSystem:
    def log_execution(self, input, output, success, user_rating):
        self.db.save({
            'input': input,
            'output': output,
            'success': success,
            'user_rating': user_rating,
            'timestamp': datetime.now()
        })

    def weekly_analysis(self):
        failures = self.db.query(success=False, last_n_days=7)
        patterns = self.analyze_patterns(failures)

        for pattern in patterns:
            logger.info(f"Common failure: {pattern}")
            # Create tickets to fix
```

---

## Testing Strategies

### Unit Test Individual Agents

```python
def test_parser_agent():
    agent = ParserAgent()

    # Test valid input
    task = agent.parse("Math homework, 2 hours, due Friday")
    assert task.name == "Math homework"
    assert task.estimated_duration_minutes == 120

    # Test missing information
    task = agent.parse("Math homework")
    assert not task.is_complete()
    assert "deadline" in task.missing_fields
```

### Integration Test Full Workflow

```python
def test_full_workflow():
    # Setup
    system = CompoundAISystem()

    # Execute
    result = system.handle_request("Math homework, 2 hours, due Friday")

    # Verify
    assert result.success
    assert len(result.schedule.events) > 0
    assert result.schedule.events[0].task_name == "Math homework"
```

### Test Failure Scenarios

```python
def test_llm_failure_recovery():
    system = CompoundAISystem()

    # Mock LLM to fail
    with mock.patch('llm.generate', side_effect=RateLimitError):
        result = system.handle_request("Task")

        # Should fallback gracefully
        assert result.error_message == "Please try again in a moment"
        assert not result.success
```

---

## Monitoring & Observability

### Key Metrics to Track

**Performance:**
- Latency per agent
- End-to-end latency
- LLM token usage
- Cost per request

**Quality:**
- Success rate
- User satisfaction (ratings)
- Override rate (how often users reject AI suggestions)
- Error rate by type

**Usage:**
- Requests per day
- Active users
- Feature adoption

### Dashboard Example

```python
class SystemDashboard:
    def get_metrics(self, last_n_days=7):
        executions = self.db.query(last_n_days=last_n_days)

        return {
            'total_requests': len(executions),
            'success_rate': self.calc_success_rate(executions),
            'avg_latency': self.calc_avg_latency(executions),
            'user_satisfaction': self.calc_avg_rating(executions),
            'cost': self.calc_total_cost(executions),
            'top_failures': self.get_top_failures(executions)
        }
```

---

## Next Steps

**Review the framework:**
- [Five-Component Framework →](Five-Component-Framework.md)
- [Multi-Agent Patterns →](Multi-Agent-Patterns.md)

**See complete example:**
- [Case Study: Planning Assistant →](Case-Study-Planning-Assistant.md)

---

[← Case Study](Case-Study-Planning-Assistant.md) | [Back to Home](Compound-AI-Home.md)
