### Problems with traditional GenAI

Given a task or a goal requiring multiple steps and decision making (eg. hiring manager JD posting example)....

Simply using traditional GenAI approaches has some problems:

1. Not reactive: LLM takes every decision based on user's query because it can't decide every step itself and there's a lot of back and forth

2. No memory: Has no memory and context

3. No specific advice: Might give generic answers and not selective based on the organization

4. Can't take actions: Can't take physical actions, just give out answers and instructions on what to do

5. Can't adapt: Won't adapt to changes or problems during a process



--> To solve 3rd issue, RAG can be used.

--> To solve 4th issue, tool calling can be used to call say APIs and do stuff on internet and servers.

--> Using Agentic AI (or AI Agents) will resolve all issues. 

Hence, this would be pro-active (autonomous), context-aware, and adaptable.


### Agentic AI

Type of AI that can take a task or a goal from user and work towards completing it on its own with minimal human guidance (humans are needed mostly for approving things).

It can plan, take actions, adapt to changes, and seeks help only when necessary.

### Key Characteristics

Autonomous, Goal oriented, Planning, Reasoning, Adaptability, Context Awareness


1. Autonomous: implemented in various ways - Execution, Decision Making, Tool Usage

Autonomy can be controlled: 

i. Permission Scope - limit what tools or actions the agent can perform independently

ii. HITL - insert checkpoints where human approval is required before continuing

iii. Override controls - allows users to stop, pause, change the agent's behavior at any time

iv. Guardrails/Policies - define hard rules or ethical boundaries for the agent

Autonomy can be dangerous: sends out incorrect details, because of some internal bias could discriminate against people, could spend a lot of money without asking, etc.

2. Goal Oriented: AI system operates with a persistent objective in mind and continuously directs its actions to achieve that, rather than responding to isolated prompts

i. Goals acts as compass for autonomy
ii. Goals can come with constraints
iii. Goals are stored in core memory (in JSON)
iv. Goals can be altered

3. Planning: agent's ability to break down a high-level goal into a structured sequence of actions of sub-goals and decide the best path to achieve desired outcome.

Essentially AI Agents do a interative 

Planning <-> Execution.

An AI Agent does planning in 3 steps:

i. Step1: Generating multiple candidate plans, Plan A, Plan B, etc.

ii. Step2: Evaluate each plan (efficiency and which is faster, tool availability, cost, risk, alignment with constraints, etc.)

iii. Step3: Select best plan with help of: HITL input (agents asks you which option you prefer), or a pre programmed policy of organization

4. Reasoning: congnitive process through which an agent interprets information, draws conclusions, and makes decisions - both while planning ahead and while executing actions in real time.


Reasoning During Planning:

i. Goal decomposition - Break down abstract goals into concrete steps
ii. Tool selection - Decide which tools will be needed for which steps
iii. Resource estimation - Estimate time, dependencies, risks

Reasoning During Execution:

i. Decision-making- Choosing between options 
ii. HITL handling - Knowing when to pause and ask for help (Unsure about salary range)
iii. Error handling - Interpreting tool/API failures and recovering

5. Adaptability: agent's ability to modify its plans, strategies, or actions in response to unexpected conditions all while staying aligned with the goal.

i. Failures (Calendar API)
ii. External feedback (Less no of applications)
iii. Changing goals (Hiring a freelancer)

6. Context-awareness: agent's ability to understand, retain, and utilize relevant information from ongoing task, past interactions, user preferences, environment cues to make better decisions through multi-step process.

i. Types of context

a. The original goal
b. Progress till now + Interaction history (Job description was finalized and posted to Linkedin & GitHub Jobs)
c. Environment state (Number of applicants so far 8 or Linkedin promotion ends in 2 days)
d. Tool responses (Resume parser "Candidate B has 3 years Django + AWS experience or Calendar API "No conflicts at 2 PM Wednesday)
e. User specific preferences (Prefers remote-first candidates or Likes receiving interview questions in a Google Doc)
f. Policy or Guardrails (Do not send offer without explicit user approval or Never use platforms that require paid ads unless approved)

ii. Context awareness is implemented through memory

iii. Short term memory

iv. Long term memory


### Components

1. Brain: Goal Interpretation, Planning, Reasoning, Tool Selection, Communication with humans

Done by LLM.

2. Orchestrator: Task Sequencing, Conditional Routing, Retry Logic, Looping and Iteration, Delegation

Done by LangGraph (or others like CrewAI or AutoGen)

3. Tools: External Actions with the world (like interacting with API call, make changes in database, send emails, etc.) and Knowledge Base Access

4. Memory: Short Term Memory (stored in Current Session), Long Term Memory, State Tracking

5. Supervisor: Approval of requests (HITL), Guardrails Enforcement, Edge Case Escalation






