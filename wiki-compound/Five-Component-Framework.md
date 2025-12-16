# The Five-Component Framework

Every effective compound AI system can be decomposed into five core components.

## Overview

The framework provides a systematic way to design and analyze AI systems:

1. **Sensing & Input** - Capture and interpret user intent
2. **Knowledge Base & Policy** - Store domain knowledge and rules
3. **Thinking & Reasoning** - Analyze and make decisions
4. **Actions** - Execute decisions and update state
5. **Evaluation & Learning** - Monitor performance and improve

---

## 1. Sensing & Input

**What it does:** Captures and interprets user intent and environmental context

### Key Responsibilities

- Parse natural language inputs
- Extract structured data from unstructured sources
- Integrate with external systems (APIs, databases)
- Validate input completeness and quality

### Example Components

- NLP parsers for user requests
- Document extraction systems
- API integrations (calendar, email, CRM)
- Input validation logic

### Design Questions

- What inputs does the system accept?
- How do we handle incomplete or ambiguous inputs?
- What external data sources are needed?
- How do we validate input quality?

### Example

```python
class InputSensor:
    def parse_user_request(self, text):
        """Parse natural language into structured task"""
        task = llm.extract_structured_data(text, schema=TaskSchema)

        if not self.is_complete(task):
            missing = self.get_missing_fields(task)
            return IncompleteInput(task, missing_fields=missing)

        return task

    def is_complete(self, task):
        required = ['name', 'duration', 'deadline']
        return all(field in task for field in required)
```

---

## 2. Knowledge Base & Policy

**What it does:** Stores domain knowledge, rules, and system state

### Key Responsibilities

- Maintain factual knowledge (RAG, vector databases)
- Store business rules and constraints
- Track system state and history
- Provide context for decision-making

### Example Components

- Vector databases for semantic search
- SQL databases for structured data
- Configuration files for business rules
- Activity logs and audit trails

### Design Questions

- What knowledge does the system need?
- How do we keep knowledge current?
- What constraints must be enforced?
- How do we handle conflicting information?

### Example

```python
class KnowledgeBase:
    def __init__(self):
        self.tasks = {}  # Pending and completed tasks
        self.calendar = {}  # Scheduled events
        self.constraints = [
            NoHomeworkAfter9PM(),
            MinimumBreakTime(15),
            WithinWorkingHours(8, 22)
        ]
        self.activity_log = []

    def get_context(self, task):
        """Gather all relevant context for a task"""
        return {
            'existing_tasks': self.get_pending_tasks(),
            'calendar_events': self.get_calendar(task.deadline),
            'constraints': self.constraints,
            'similar_past_tasks': self.find_similar(task)
        }
```

---

## 3. Thinking & Reasoning

**What it does:** Analyzes information, makes decisions, generates plans

### Key Responsibilities

- Orchestrate multiple agents or models
- Analyze feasibility and impacts
- Generate alternatives and recommendations
- Apply domain-specific reasoning

### Example Components

- LLM agents for reasoning tasks
- Planning algorithms
- Optimization engines
- Multi-agent orchestrators

### Design Questions

- What decisions need to be made?
- Which tasks need AI vs traditional logic?
- How do components coordinate?
- What happens when agents disagree?

### Example

```python
class ThinkingLayer:
    def analyze_task(self, task, context):
        """Analyze feasibility and generate plan"""

        # Check for conflicts
        conflicts = self.conflict_detector.find_conflicts(
            task, context['calendar_events']
        )

        if conflicts:
            # Generate alternatives
            alternatives = self.planner.generate_alternatives(
                task, context, avoid_conflicts=conflicts
            )
            return {
                'feasible': False,
                'conflicts': conflicts,
                'alternatives': alternatives
            }

        # Generate optimal plan
        plan = self.planner.generate_plan(task, context)

        return {
            'feasible': True,
            'plan': plan,
            'reasoning': self.explain_plan(plan)
        }
```

---

## 4. Actions

**What it does:** Executes decisions and updates state

### Key Responsibilities

- Make changes to external systems
- Update databases and knowledge base
- Trigger workflows and notifications
- Handle rollbacks on failure

### Example Components

- API clients for external services
- Database write operations
- Email/notification systems
- Transaction managers

### Design Questions

- What actions can the system take?
- How do we ensure safe execution?
- What requires human approval?
- How do we handle failures?

### Example

```python
class ActionExecutor:
    def execute_plan(self, plan, approved=False):
        """Execute approved plan with rollback on failure"""

        if not approved:
            raise Exception("Plan must be approved before execution")

        # Transaction pattern for safety
        try:
            # Update calendar
            for event in plan.events:
                self.calendar.create_event(event)

            # Update task status
            self.knowledge_base.mark_scheduled(plan.task_id)

            # Send notifications
            self.notify_user(plan)

            return ExecutionSuccess(plan)

        except Exception as e:
            # Rollback changes
            self.rollback(plan)
            return ExecutionFailure(plan, error=e)
```

---

## 5. Evaluation & Learning

**What it does:** Monitors performance, detects errors, enables improvement

### Key Responsibilities

- Validate outputs before execution
- Monitor system performance
- Collect user feedback
- Enable self-improvement loops

### Example Components

- Output validators
- Quality scoring systems
- User feedback collection
- A/B testing infrastructure
- Performance dashboards

### Design Questions

- How do we know if outputs are good?
- What metrics matter?
- How do we collect feedback?
- How does the system improve over time?

### Example

```python
class EvaluationLayer:
    def validate_plan(self, plan, constraints):
        """Validate plan meets all constraints"""

        for event in plan.events:
            # Time bounds
            if not within_working_hours(event):
                return ValidationFailure("Outside working hours")

            # Conflicts
            if has_overlap(event, existing_events):
                return ValidationFailure("Schedule conflict")

            # Constraints
            for constraint in constraints:
                if constraint.violated_by(event):
                    return ValidationFailure(f"Violates {constraint}")

        return ValidationSuccess()

    def learn_from_completion(self, task, actual_duration):
        """Update estimates based on actual data"""

        error = actual_duration - task.estimated_duration

        # Log for analysis
        self.performance_log.append({
            'task_type': task.category,
            'estimated': task.estimated_duration,
            'actual': actual_duration,
            'error': error
        })

        # Generate insights
        if abs(error) > 15 minutes:
            return f"Consider {abs(error)} min buffer for {task.category}"
```

---

## Putting It All Together

### Example Flow

```python
def handle_user_request(user_input):
    # 1. SENSING
    task = input_sensor.parse(user_input)
    if task.incomplete:
        return request_more_info(task.missing_fields)

    # 2. KNOWLEDGE
    context = knowledge_base.get_context(task)

    # 3. THINKING
    analysis = thinking_layer.analyze_task(task, context)

    if not analysis['feasible']:
        # Present alternatives to user
        choice = presenter.get_user_choice(analysis['alternatives'])
        plan = choice
    else:
        plan = analysis['plan']

    # 4. EVALUATION (before execution)
    validation = evaluator.validate_plan(plan, context['constraints'])
    if not validation.success:
        return validation.error

    # Get human approval
    if not user_approves(plan):
        return None

    # 5. ACTIONS
    result = action_executor.execute_plan(plan, approved=True)

    # 6. EVALUATION (after execution)
    evaluator.log_execution(user_input, plan, result)

    return result
```

---

## Key Principles

**1. Clear Separation of Concerns**
Each component has distinct responsibilities

**2. Structured Data Flow**
Components pass structured data, not just text

**3. Validation at Boundaries**
Check inputs and outputs at each stage

**4. Observability**
Log decisions at each component

**5. Feedback Loops**
Learn from execution to improve future decisions

---

## Next Steps

**See architecture patterns:**
- [Multi-Agent Patterns →](Multi-Agent-Patterns.md)

**Learn implementation:**
- [Implementation Guide →](Implementation-Guide.md)

**Study real example:**
- [Case Study: Planning Assistant →](Case-Study-Planning-Assistant.md)

---

[← What Are Compound AI Systems?](What-Are-Compound-AI-Systems.md) | [Next: Multi-Agent Patterns →](Multi-Agent-Patterns.md)
