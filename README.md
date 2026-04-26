# Learning LangGraph

I am currently exploring LangGraph to build stateful, multi-step LLM workflows — going beyond simple chains to graph-based orchestration that supports parallelization, conditional routing, iterative feedback loops, and production-grade state persistence.

This builds directly on my prior [LangChain work](https://github.com/suryam1504/Learning-LangChain) and focuses on the patterns that LangChain alone cannot handle cleanly: loops, fault tolerance, and human-in-the-loop workflows.

---

## Workflow Patterns Explored

### Foundational Graphs and State (`1_simple_workflow.ipynb`)
- Defined state schemas using `TypedDict` and built directed graphs with `StateGraph`
- Implemented a multi-node BMI calculator to understand how state flows through and gets updated at each node
- Visualized the compiled graph to understand the execution topology

### LLM Integration (`2_simple_llm_workflow.ipynb`)
- Integrated `ChatOpenAI` (GPT-4.1-nano) into a graph node for a basic Q&A workflow
- Understood how to manage prompts and LLM responses as first-class state in a graph

### Prompt Chaining (`3_prompt_chaining_workflow.ipynb`)
- Built a sequential two-node pipeline: generate outline → generate full blog post
- Key insight over LangChain chains: intermediate outputs (e.g., the outline) remain accessible in state throughout the entire workflow, enabling inspection and debugging at every step

### Parallel Execution (`4_simple_parallel_workflow.ipynb`, `5_complex_parallel_workflow.ipynb`)
- **Simple**: Fan-out from a single node to multiple parallel nodes, then fan-in to an aggregator — built a cricket statistics calculator (strike rate, balls per boundary, boundary percentage) running concurrently
- **Complex with Reducers**: Used `Annotated[list[int], operator.add]` to safely accumulate outputs from parallel nodes into a single state key — built a UPSC essay evaluator where three nodes independently score Chain of Thought, Depth of Analysis, and Language quality, then aggregate into an overall score
- Understood the `InvalidUpdateError` problem and how returning partial state (only the keys a node touches) avoids conflicts in parallel execution

### Conditional Branching (`6_simple_conditional_workflow.ipynb`, `7_complex_conditional_workflow.ipynb`)
- **Deterministic routing**: Used `add_conditional_edges()` with a router function and `Literal` return types to branch a quadratic equation solver into three paths based on discriminant value
- **LLM-driven routing**: Built a customer review response system where an LLM classifies sentiment, routes positive reviews directly to a thank-you response, and routes negative reviews through a structured diagnosis step (issue type, tone, urgency) before generating a tailored empathetic reply — demonstrating dynamic multi-path workflows where routing decisions are made by the model, not hardcoded logic

### Iterative Feedback Loops (`8_iterative_workflow.ipynb`)
- Built a Twitter post generator with a generate → evaluate → optimize loop
- The LLM evaluator grades tweets on originality, humor, punchiness, and virality; if rejected, an optimizer regenerates using the specific feedback
- Used `Annotated[list[str], operator.add]` to track the full history of tweet drafts and feedback across iterations
- Implemented a max-iteration guard in the router to prevent infinite loops
- This demonstrates the **Evaluator-Optimizer** agentic pattern

### State Persistence and Fault Tolerance (`9_basic_chatbot.ipynb`, `10_persistence.ipynb`)
- **The problem**: Without persistence, each `invoke()` call discards state — multi-turn conversations are impossible
- **Checkpointing**: Used `InMemorySaver` to snapshot state at every superstep of the graph, enabling full state history per thread
- **Threads**: Analogous to separate chat sessions — each `thread_id` maintains its own independent state history on the same compiled graph (e.g., "build_resume" and "write_cover_letter" run independently)
- **Time travel**: Retrieved any past checkpoint by `checkpoint_id` and replayed execution from that point, creating a new branch in the history tree — useful for A/B testing different LLM outputs from the same starting state
- **State updates**: Used `.update_state()` to directly modify state at a checkpoint and resume — e.g., changing a joke topic mid-workflow without restarting
- **Fault tolerance**: Detected mid-execution failures, retrieved the last successful checkpoint, and resumed from exactly where the workflow crashed — no restart needed

---

## Key LangGraph Concepts

| Concept | What it enables |
|---|---|
| `StateGraph` + `TypedDict` | Typed, structured shared memory across all nodes |
| Reducers (`Annotated` + `operator.add`) | Safe state aggregation from parallel nodes |
| `add_conditional_edges()` | Branching and looping without messy if-else chains |
| `.with_structured_output()` | Reliable, schema-validated LLM responses using Pydantic |
| `InMemorySaver` / Checkpointers | State snapshots for persistence, recovery, and replay |
| Threads (`thread_id`) | Independent session management for multi-user workflows |
| Time Travel | Replay any prior checkpoint to branch or debug workflows |

---

## Why LangGraph over LangChain

LangChain is excellent for linear pipelines, but breaks down when workflows need:
- **Loops** — no native cycle support in LCEL chains
- **Conditional branching** — possible but awkward without graph structure
- **State persistence** — chains complete and discard state; no checkpointing
- **Fault tolerance** — no mid-workflow recovery without external scaffolding
- **Human-in-the-Loop** — no pause/resume mechanism

LangGraph addresses all of these natively through its graph model, stateful execution, and checkpointing infrastructure.

---

## Agentic AI Patterns Demonstrated

- **Prompt Chaining** — sequential LLM calls where each builds on the last
- **Routing** — LLM classifies input and directs workflow to the appropriate path
- **Parallelization** — independent subtasks execute concurrently and merge
- **Evaluator-Optimizer** — iterative refinement loop with LLM-as-judge
- **Fault Tolerance** — resume from the last successful checkpoint after failure

---

## Tech Stack

- **LangGraph** (1.1.9) — graph orchestration, state management, checkpointing
- **LangChain + LangChain-OpenAI** — LLM interface and prompt management
- **OpenAI API** — GPT-4 series models
- **Pydantic** — structured output schemas and validation
- **Python** (`TypedDict`, `Annotated`, `operator`) — state type definitions and reducers
