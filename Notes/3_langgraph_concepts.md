## LLM Workflow

1. LLM workflows are a step by step process using which we can build complex LLM applications.

2. Each step in a workflow performs a distinct task - such as prompting, reasoning, tool calling, memory access, or decision-making.

3. Workflows can be linear, parallel, branched, or looped, allowing for complex behaviours like retries, multi-agent communication, or tool-augmented reasoning.


### Common workflows

Ref - https://docs.langchain.com/oss/python/langgraph/workflows-agents

1. Prompt Chaining

Calling LLMs multiple times in sequence. Some pass fail checks can be inserted in between.

2. Routing

An LLM decides/routes which LLM will be used for further processing.

A customer chatbot LLM decides if user's query should go to sales LLM or exchange item LLM or refund LLM.

3. Parallelization

A given task is driven into multiple sub tasks, each sub task is processed by different LLMs, then you aggregated and return result.

5. Orchestrator Workers

Very similar to Parallelization, but here, we don't know the nature of what LLM of each sub task is going to do and all is decided dynamically while process is going on.

6. Evaluator Optimizer

Task --> LLM (Solution) <--> LLM (Reject+Feedback) (this keeps going on iteratively till we get accepted result) --> Out


## Graphs, Nodes, Edges

Same stuff, The goal and the steps needed to complete it are presented in the form of a graph.

Every task is a node.

Edges decide control flow. (done by agents autonomously hence complex workflows can be completed too)


## State

State is the shared memory that flows through your workflow - it holds all data being passsed between nodes and edges as the graph runs. 

Every node has access to it and is mutable.

Stored in forms of key value pair.


## Reducers

defines how updates from Nodes are applied to the shared state.

Each key in the state can have its own reducer, which determines whether new data replaces, merges, or adds to the existing value. 



## LangGraph Execution Model

1. Graph Definition

You define:

- The state schema
- Nodes (functions that perform tasks)
- Edges (which node connects to which)

2. Compilation

You call .compile() on the StateGraph.

This checks the graph structure and prepares it for execution.

3. Invocation

You run the graph with .invoke(initial_state). LangGraph sends the initial state as a message to the entry node(s).

4. Super-Steps Begin

Execution proceeds in rounds.

In each round (super-step):

All active nodes (those that received messages) run in parallel

Each returns an update (message) to the state

5. Message Passing & Node Activation

The messages are passed to downstream nodes via edges. Nodes that receive messages become active for the next round.

6. Halting Condition

Execution stops when:

- No nodes are active, and
- No messages are in transit