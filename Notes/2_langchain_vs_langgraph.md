## LangChain

LangChain is an open-source library designed to simplify the process of building LLM based applications.

It provides modular building blocks that let you create sophisticated LLM-based workflows with ease.

LangChain consists of multiple components:

1. **Model** components gives use a unified interface to interact with various LLM providers
2. **Prompts** component helps you engineer prompts
3. **Retrievers** component helps you fetch relevant documents from a vector store

But the biggest offering of LangChain is **Chains.**

What can you do with LangChain:

1. Simple conversational workflows (pre-defined code paths) like Chatbots, Text Summarizers
2. Multistep workflows
3. RAG applications
4. Basic level agents

### Challenges in workflows if LangChain is used

1. Control Flow Complexity: Conditional Branch, Loops, Jumps in flowcharts

2. Handling State: in a complex workflow, at every step, that step has a state which has some information (like in JD example a task could have a state with values about JD_approved, JD_posted as True or False, min_applicants, num_applications, etc.). This info changes dynamically as the process keeps going on. These are stored in a key value pair, which langchain can't handle very well, it just operates on pure textual input.

3. Event Driven Execution; workflows can be sequential or event driven. Sequential ones have a step by step process which completes in one flow without stopping. Event driven could have cases where the workflow needs to be paused for a while and only resumes based on user's response (after posting JD workflow needs to wait for 7 days before automatic screening of resumes, or after sending an offer letter agent needs to wait for their decision, etc.). LangChain simply can't do all this. 

4. Fault Tolerance: if something fails, can it get back? complex workflows often run into this when things go on for days and weeks. Could be of two types:

i. small: node level fault, some little API failure when trying to do something

ii. big: server down, etc.

With LangChain, no concept of fault tolerance, so if it breaks once, it will restart from beginning and won't ideally resume from where it stopped.

5. Human in the Loop: waiting for a human to approve something and then continue afterwards is not a default concept in LangChain and would be a problem in long running workflows.

6. Nested Workflows: just inherently difficult with LangChain.

7. Observability: monitoring, debugging, and understanding what your workflow is doing at runtime. LangSmith does this and it can indeed be easily linked with LangChain, the problem is: when trying to code these complex workflows with LangChain, not everything would be possible with LangChain syntax and we would end up writing some "glue code" (personalized functions and codes with our own if-else or while loops, etc.). Now LangSmith can only track LangChain syntaxes, not our personal functions. 



## LangGraph

Here,

1. The goal and the steps needed to complete it are presented in the form of a graph.

Every task is a node.

Edges decide control flow. (done by agents autonomously hence complex workflows can be completed too)

So you essentially skip a lot of if-else and while loops which would have been otherwise used with LangChain. 

2. When creating a graph, a state object is created (can be created in Pydantic or TypeDict, essentially its a dictionary). Every node has access to this object and is mutable, so as the workflow goes on, any node has ability to mutate it and things work appropriately and workflow is smooth.

This feature of LangGraph is called being **stateful**.

3. LangGraph inherently and by design provides event driven execution. 

At a certain node, you can use this "checkpointer" feature to save the state at that point (either in memory or in a database), pause and wait for an external trigger, retrieve the state and just continue.

4. LangGraph handles fault tolerance with retry logics for small level faults and recovery concepts for large level faults.

5. HITL: LangGraph could do this with persistence execution state and saving states with checkpointer, etc. to pause this for days weeks months.

6. Since this is based on graphs, a node could have sub-graphs and hence nested workflows are simple here.

7. LangSmith and LangGraph works very well together. A nice timeline is made from start to end.


Finally,

1. What is LangGraph?

LangGraph is an orchestration framework that enables you to build stateful, multi-step, and event-driven workflows using large language models (LLMs). It's ideal for designing both single-agent and multi-agent agentic Al applications.

Think of LangGraph as a flowchart engine for LLMs you define the steps (nodes), how they're connected (edges), and the logic that governs the transitions. LangGraph takes care of state management, conditional branching, looping, pausing/resuming, and fault recovery features essential for building robust, production-grade Al systems.

2. When to Use What?

Use LangChain when you're building simple, linear workflows like a prompt chain, summarizer, or a basic retrieval system.

Use LangGraph when your use case involves complex, non-linear workflows that need:

- Conditional paths
- Loops
- Human-in-the-loop steps
- Multi-agent coordination
- Asynchronous or event-driven execution