# Case Study: AI Planning Assistant

A complete example of building a compound AI system for task scheduling.

## Problem Statement

Build a system to help students manage homework and activities by:
- Understanding natural language task descriptions
- Detecting scheduling conflicts automatically
- Proposing optimal schedules
- Learning from completion patterns to improve estimates

### User Experience Goal

**Instead of:**
"I have math homework due Friday. Let me open my calendar, check what's there, find 2 hours of free time, avoid my soccer practice, make sure it's not too late..."

**User types:**
"Math homework, 2 hours, due Friday"

**System responds:**
"I've scheduled your math homework for Wednesday 3-5 PM, avoiding your soccer practice and giving you a buffer day before the deadline. Approve?"

---

## System Design

### 1. Five-Component Mapping

**Sensing & Input:**
- Natural language parser (LLM-based)
- Calendar API integration
- Input validation (check for missing fields)

**Knowledge Base & Policy:**
- Task database (pending, completed)
- Calendar events
- Constraints: no homework after 9 PM, minimum 15min breaks
- Historical completion data for learning

**Thinking (5 Specialized Agents):**
- User Interaction Agent - Parse requests
- Impact Generator Agent - Detect conflicts
- Plan Writer Agent - Generate schedules
- User Confirmation Agent - Present options
- Delivery Agent - Execute changes

**Actions:**
- Create calendar events
- Update task statuses
- Send notifications

**Evaluation:**
- Validate schedules before execution
- Track estimated vs actual duration
- Recommend buffer adjustments

---

### 2. Agent Architecture

We use the **Orchestrator-Worker** pattern with **Human-in-the-Loop**.

```
User Input
    ↓
Parser Agent → Structured Task
    ↓
Impact Generator → Conflict Analysis
    ↓
Plan Writer → Schedule Options
    ↓
User Confirmation → Approved Plan
    ↓
Delivery Agent → Updated Calendar
```

#### Agent Specifications

| Agent | Input | Output | LLM? |
|-------|-------|--------|------|
| **Parser** | "Math homework, 2 hours, due Friday" | Task(name="Math homework", duration=120, deadline=Friday) | Yes |
| **Impact Generator** | Task + Calendar | FeasibilityReport(conflicts=[], available_slots=[...]) | Partial |
| **Plan Writer** | Task + Available slots | Schedule(events=[...], reasoning="...") | Yes |
| **User Confirmation** | Schedule options | ApprovedPlan or Rejection | No (UI) |
| **Delivery** | Approved plan | ExecutionResult(success=True) | No |

---

### 3. Implementation Details

#### Data Models

```python
@dataclass
class Task:
    task_id: str
    name: str
    description: str
    estimated_duration_minutes: int
    deadline: datetime
    priority: int = 5
    status: TaskStatus = TaskStatus.PENDING

@dataclass
class CalendarEvent:
    event_id: str
    task_id: str
    start_time: datetime
    end_time: datetime
    task_name: str
    status: str = "scheduled"

@dataclass
class Constraint:
    name: str
    description: str

    def violated_by(self, event: CalendarEvent) -> bool:
        raise NotImplementedError

class NoHomeworkAfter9PM(Constraint):
    def violated_by(self, event: CalendarEvent) -> bool:
        return event.end_time.hour >= 21
```

#### Agent 1: Parser

```python
class ParserAgent:
    def parse(self, user_input: str) -> Task:
        """Extract structured task from natural language"""

        prompt = f'''
        Extract task details from: "{user_input}"

        Return JSON with:
        - name: task name
        - duration: estimated minutes
        - deadline: when it's due
        - priority: 1-10 (default 5)
        '''

        response = llm.generate(prompt, response_format="json")

        task = Task(
            task_id=uuid.uuid4(),
            name=response['name'],
            estimated_duration_minutes=response['duration'],
            deadline=parse_date(response['deadline']),
            priority=response.get('priority', 5)
        )

        return task
```

#### Agent 2: Impact Generator

```python
class ImpactGeneratorAgent:
    def analyze(self, task: Task, context: PlanningContext):
        """Detect conflicts and assess feasibility"""

        # Find all events near the deadline
        relevant_events = self.get_events_before(
            task.deadline,
            lookback_days=7
        )

        # Check for conflicts
        conflicts = []
        for event in relevant_events:
            if self.would_conflict(task, event):
                conflicts.append(event)

        # Find available time slots
        available_slots = self.find_available_slots(
            task.estimated_duration_minutes,
            before=task.deadline,
            constraints=context.constraints
        )

        return FeasibilityReport(
            task=task,
            conflicts=conflicts,
            available_slots=available_slots,
            feasible=len(available_slots) > 0
        )
```

#### Agent 3: Plan Writer

```python
class PlanWriterAgent:
    def generate_plan(self, task: Task, feasibility: FeasibilityReport):
        """Generate optimal schedule"""

        if not feasibility.feasible:
            return self.generate_alternatives(task, feasibility)

        # Select best time slot
        best_slot = self.select_best_slot(
            feasibility.available_slots,
            task.priority
        )

        # Create schedule
        schedule = Schedule(
            task=task,
            events=[CalendarEvent(
                event_id=uuid.uuid4(),
                task_id=task.task_id,
                start_time=best_slot.start,
                end_time=best_slot.start + timedelta(
                    minutes=task.estimated_duration_minutes
                ),
                task_name=task.name
            )],
            reasoning=self.explain_choice(best_slot, task)
        )

        return schedule

    def explain_choice(self, slot, task):
        """Explain why this time was chosen"""
        return f'''
        Scheduled for {slot.start.strftime("%A %I:%M %p")} because:
        - Avoids your existing commitments
        - Gives you {(task.deadline - slot.end).days} day buffer
        - During your productive hours
        - Respects no-homework-after-9PM constraint
        '''
```

#### Agent 4: User Confirmation

```python
class UserConfirmationAgent:
    def get_approval(self, schedule: Schedule) -> bool:
        """Present schedule to user and get approval"""

        print(f"\n📅 Proposed Schedule for: {schedule.task.name}")
        print(f"⏰ Time: {schedule.events[0].start_time.strftime('%A, %I:%M %p')}")
        print(f"⏱️  Duration: {schedule.task.estimated_duration_minutes} minutes")
        print(f"\n💡 Reasoning:")
        print(schedule.reasoning)

        response = input("\nApprove this schedule? (yes/no): ")
        return response.lower() == 'yes'
```

#### Agent 5: Delivery

```python
class DeliveryAgent:
    def execute(self, schedule: Schedule, approved: bool):
        """Execute the approved schedule"""

        if not approved:
            raise Exception("Cannot execute unapproved schedule")

        # Create calendar events
        for event in schedule.events:
            self.calendar.create_event(event)

        # Update task status
        self.knowledge_base.mark_scheduled(schedule.task.task_id)

        # Log for learning
        self.performance_tracker.log_schedule(schedule)

        return ExecutionResult(
            success=True,
            schedule=schedule,
            message=f"Scheduled {schedule.task.name}"
        )
```

---

### 4. Orchestration

```python
def handle_task_request(user_input: str):
    # 1. PARSE
    task = parser_agent.parse(user_input)

    # Validate completeness
    if not task.is_valid():
        return request_missing_info(task)

    # 2. GET CONTEXT
    context = knowledge_base.get_context(task)

    # 3. ANALYZE
    feasibility = impact_generator.analyze(task, context)

    # 4. PLAN
    if feasibility.has_conflicts():
        alternatives = plan_writer.generate_alternatives(task, feasibility)
        schedule = user_confirmation.choose_from_alternatives(alternatives)
    else:
        schedule = plan_writer.generate_plan(task, feasibility)

    # 5. VALIDATE
    validation = validator.validate_schedule(schedule, context.constraints)
    if not validation.success:
        return validation.error

    # 6. HUMAN APPROVAL
    approved = user_confirmation.get_approval(schedule)

    if not approved:
        return "Schedule rejected by user"

    # 7. EXECUTE
    result = delivery_agent.execute(schedule, approved=True)

    return result
```

---

### 5. Quality Gates

**Input Validation:**
```python
def validate_task(task):
    if not task.name:
        return False, "Task must have a name"

    if task.estimated_duration_minutes <= 0:
        return False, "Duration must be positive"

    if task.deadline < datetime.now():
        return False, "Deadline is in the past"

    return True, None
```

**Schedule Validation:**
```python
def validate_schedule(schedule, constraints):
    for event in schedule.events:
        # Check working hours
        if event.start_time.hour < 8 or event.end_time.hour > 22:
            return ValidationFailure("Outside working hours")

        # Check constraints
        for constraint in constraints:
            if constraint.violated_by(event):
                return ValidationFailure(f"Violates: {constraint.name}")

        # Check conflicts
        if has_overlap(event, existing_events):
            return ValidationFailure("Schedule conflict")

    return ValidationSuccess()
```

---

### 6. Self-Learning Loop

```python
class SelfLearningSystem:
    def track_completion(self, task_id, actual_duration):
        """Learn from completed tasks"""

        task = self.knowledge_base.get_task(task_id)

        estimation_error = actual_duration - task.estimated_duration_minutes

        self.learning_log.append({
            'task_category': task.category,
            'estimated': task.estimated_duration_minutes,
            'actual': actual_duration,
            'error': estimation_error,
            'error_percent': estimation_error / task.estimated_duration_minutes
        })

    def get_insights(self):
        """Analyze patterns and generate recommendations"""

        by_category = {}
        for log in self.learning_log:
            category = log['task_category']
            if category not in by_category:
                by_category[category] = []
            by_category[category].append(log['error_percent'])

        insights = []
        for category, errors in by_category.items():
            avg_error = sum(errors) / len(errors)

            if avg_error > 0.20:  # 20% underestimation
                insights.append(
                    f"Add 25% buffer for {category} tasks"
                )

        return insights
```

---

## Results & Learnings

### What Works Well

✅ **Natural language understanding** - Users can type casual requests

✅ **Conflict detection** - Automatically avoids scheduling conflicts

✅ **Alternative generation** - Proposes options when conflicts exist

✅ **Constraint satisfaction** - Respects all defined rules

✅ **Transparency** - Explains scheduling decisions

✅ **Human control** - User approves before changes

✅ **Continuous learning** - Improves estimates over time

### Design Decisions Explained

**Why 5 agents instead of 1 LLM?**
- Single LLM would mix parsing, reasoning, and execution
- Specialized agents are easier to test and debug
- Can improve components independently
- Can use different models (GPT-3.5 for parsing, GPT-4 for planning)

**Why human-in-the-loop?**
- Builds user trust
- Prevents errors from reaching calendar
- User maintains control over schedule

**Why track estimation accuracy?**
- Enables self-improvement
- Identifies consistent under/over-estimation patterns
- Provides actionable feedback

---

## Key Takeaways

1. **Decomposition is critical** - Breaking into 5 agents made development manageable

2. **Validation at boundaries** - Catching errors early prevents propagation

3. **Human approval matters** - Users need control over their schedule

4. **Logging enables learning** - Can't improve what you don't measure

5. **Start simple, iterate** - Built basic version first, added self-learning later

---

## Next Steps

**Learn best practices:**
- [Best Practices →](Best-Practices-Compound-AI.md)

**Review the framework:**
- [Five-Component Framework →](Five-Component-Framework.md)
- [Multi-Agent Patterns →](Multi-Agent-Patterns.md)

---

[← Implementation Guide](Implementation-Guide.md) | [Next: Best Practices →](Best-Practices-Compound-AI.md)
