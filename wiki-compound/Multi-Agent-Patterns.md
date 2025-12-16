# Multi-Agent Architecture Patterns

Common patterns for organizing multiple AI agents.

## Overview

When building compound AI systems with multiple agents, certain architectural patterns emerge. Choosing the right pattern depends on your task structure and requirements.

---

## Pattern 1: Sequential Pipeline

Agents execute in a fixed order, each building on previous outputs.

```
Input → Agent 1 → Agent 2 → Agent 3 → Output
```

### When to Use

- **Clear sequential dependencies** - Each step must complete before the next
- **Data transformation** - Each step transforms data for the next stage
- **Linear workflow** - No branching or parallel paths needed

### Example: Document Processing

```
Document → OCR Agent
         → Layout Analysis Agent
         → Field Extraction Agent
         → Validation Agent
         → Structured Data
```

### Implementation

```python
def sequential_pipeline(document):
    # Stage 1: OCR
    text = ocr_agent.extract_text(document)

    # Stage 2: Layout Analysis
    layout = layout_agent.analyze(text)

    # Stage 3: Field Extraction
    fields = extraction_agent.extract_fields(text, layout)

    # Stage 4: Validation
    validated = validation_agent.validate(fields)

    return validated
```

### Pros & Cons

✅ Simple to understand and debug
✅ Easy to add stages
✅ Clear failure points

❌ No parallelization
❌ Slowest stage bottlenecks entire pipeline
❌ Errors propagate downstream

---

## Pattern 2: Orchestrator-Worker

Central orchestrator delegates tasks to specialized workers.

```
         ┌─→ Worker 1 ─┐
Input → Orchestrator → Worker 2 → Aggregator → Output
         └─→ Worker 3 ─┘
```

### When to Use

- **Multiple independent subtasks** - Tasks can run in parallel
- **Specialized expertise** - Each worker handles different domain
- **Need central coordination** - Orchestrator decides which workers to use

### Example: Research Assistant

```
Query → Orchestrator
         ├→ Web Search Worker
         ├→ Document Retrieval Worker
         └→ Code Search Worker
       → Synthesis Agent → Report
```

### Implementation

```python
class Orchestrator:
    def process_query(self, query):
        # Decide which workers needed
        task_plan = self.plan_tasks(query)

        # Dispatch to workers (parallel)
        results = await asyncio.gather(
            self.web_worker.search(query) if 'web' in task_plan else None,
            self.doc_worker.retrieve(query) if 'docs' in task_plan else None,
            self.code_worker.search(query) if 'code' in task_plan else None
        )

        # Synthesize results
        report = self.synthesis_agent.synthesize(results)

        return report
```

### Pros & Cons

✅ Parallelization for speed
✅ Flexible - orchestrator adapts to task
✅ Easy to add new workers

❌ Orchestrator complexity
❌ Need result aggregation logic
❌ Harder to debug

---

## Pattern 3: Critic-Revise Loop

Generator creates output, critic evaluates, iterate until quality met.

```
Generator → Output → Critic → [Pass] → Final Output
                       ↓
                    [Fail]
                       ↓
                   Reviser → Generator
```

### When to Use

- **Quality must meet high standards** - Iterative refinement improves results
- **Clear quality criteria** - Critic can objectively evaluate
- **Cost of iteration acceptable** - Worth multiple LLM calls

### Example: Code Generation

```
Generate Code → Code Reviewer (Critic)
                    ↓
              [Issues Found]
                    ↓
              Code Refiner → [Generate Again]
                    ↓
              [No Issues]
                    ↓
              Final Code
```

### Implementation

```python
def critic_revise_loop(prompt, max_iterations=3):
    for i in range(max_iterations):
        # Generate
        code = generator_agent.generate_code(prompt)

        # Critique
        review = critic_agent.review(code)

        if review.acceptable:
            return code

        # Revise based on feedback
        prompt = refiner_agent.refine_prompt(prompt, review.issues)

    # Return best attempt
    return code
```

### Pros & Cons

✅ Higher quality outputs
✅ Self-correcting
✅ Can catch subtle errors

❌ Multiple LLM calls (expensive)
❌ May not converge
❌ Need max iteration limit

---

## Pattern 4: Human-in-the-Loop

Critical decisions require human approval before execution.

```
AI Analysis → Present Options → Human Decision → Execute
```

### When to Use

- **High-stakes decisions** - Errors have serious consequences
- **Ethical/legal considerations** - Human accountability required
- **Building trust** - Users need control over decisions
- **Ambiguous situations** - AI confidence is low

### Example: Medical Diagnosis

```
Symptoms → AI Diagnosis Agent
         → Present to Doctor (with confidence scores)
         → Doctor Reviews & Decides
         → Treatment Plan
```

### Implementation

```python
def human_in_loop_workflow(input_data):
    # AI analyzes and generates options
    analysis = ai_agent.analyze(input_data)
    options = ai_agent.generate_options(analysis)

    # Present to human with reasoning
    presentation = {
        'options': options,
        'reasoning': analysis.reasoning,
        'confidence': analysis.confidence,
        'risks': analysis.identified_risks
    }

    # Wait for human decision
    human_choice = await get_human_decision(presentation)

    if human_choice == 'override':
        # Human provides custom decision
        decision = human_choice.custom_input
    else:
        decision = options[human_choice.selected]

    # Execute approved decision
    return executor.execute(decision, approved=True)
```

### Pros & Cons

✅ Safety and accountability
✅ Builds user trust
✅ Catches AI errors

❌ Not fully automated
❌ Latency (waiting for human)
❌ Requires good UX for decision presentation

---

## Pattern 5: Hierarchical Agents

Agents organized in a hierarchy - managers coordinate sub-agents.

```
          Manager Agent
          /     |     \
    Agent A  Agent B  Agent C
              / | \
         Sub-B1 Sub-B2 Sub-B3
```

### When to Use

- **Complex, nested tasks** - Tasks naturally decompose hierarchically
- **Need task delegation** - High-level agents break down work
- **Different abstraction levels** - Strategic vs tactical agents

### Example: Software Development

```
Product Manager Agent
    ├→ Backend Dev Agent
    │   ├→ API Designer
    │   └→ Database Agent
    ├→ Frontend Dev Agent
    │   ├→ UI Designer
    │   └→ Component Builder
    └→ QA Agent
        ├→ Unit Test Agent
        └→ Integration Test Agent
```

### Pros & Cons

✅ Natural task decomposition
✅ Clear responsibilities
✅ Scalable to complex tasks

❌ Communication overhead
❌ Deep hierarchies are slow
❌ Coordination complexity

---

## Choosing the Right Pattern

| Pattern | Use When | Avoid When |
|---------|----------|------------|
| **Sequential Pipeline** | Linear dependencies, data transformation | Need parallelism, no clear sequence |
| **Orchestrator-Worker** | Independent parallel tasks | Tasks have dependencies |
| **Critic-Revise** | Quality critical, clear criteria | Cost-sensitive, time-sensitive |
| **Human-in-Loop** | High stakes, need oversight | Need full automation |
| **Hierarchical** | Complex nested tasks | Simple workflows |

---

## Combining Patterns

Real systems often combine multiple patterns:

### Example: Customer Support System

```
User Query
    ↓
Orchestrator (Pattern 2)
    ├→ Intent Classifier
    ├→ Knowledge Retrieval
    └→ Sentiment Analyzer
    ↓
Response Generator (Pattern 3: Critic-Revise)
    → Draft Response
    → Quality Critic
    → Revise if needed
    ↓
Escalation Decision (Pattern 4: Human-in-Loop)
    → If complex/sensitive: Route to human
    → If standard: Auto-respond
```

---

## Best Practices

**1. Start Simple**
Begin with Sequential Pipeline, evolve to more complex patterns as needed

**2. Limit Depth**
Avoid deep nesting (>3 levels) - gets hard to debug

**3. Parallelize Wisely**
Only parallelize truly independent tasks

**4. Set Iteration Limits**
Critic-Revise loops must have max iterations

**5. Log Agent Interactions**
Track which agents were called and what they returned

**6. Design for Observability**
Make it easy to see what each agent is doing

---

## Next Steps

**Learn how to implement:**
- [Implementation Guide →](Implementation-Guide.md)

**See real example:**
- [Case Study: Planning Assistant →](Case-Study-Planning-Assistant.md)

**Read best practices:**
- [Best Practices →](Best-Practices-Compound-AI.md)

---

[← Five-Component Framework](Five-Component-Framework.md) | [Next: Implementation Guide →](Implementation-Guide.md)
