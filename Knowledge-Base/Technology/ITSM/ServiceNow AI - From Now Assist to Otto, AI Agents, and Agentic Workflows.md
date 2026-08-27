Source: #source/internet_resources 
Project: #project/personal 
Areas: #area/work
Subject: #subject/professional 
Type: #type/article 
Learning priority: #priority/P3 
Status: #status/in_process
Related: 

---
# ServiceNow AI: From Now Assist to Otto, AI Agents, and Agentic Workflows

[How to Activate Your Now Assist for ITSM](https://www.youtube.com/watch?v=RUh5mYXbDLA&t=320s)

## Introduction

ServiceNow is going through a major shift in how work is performed on its platform.

Historically, ServiceNow automation was primarily based on structured processes:

**User → Form → Workflow → Automation → Result**

With generative AI and agentic AI, the model is evolving toward:

**User → Intent → AI → Agent → Tools → Workflow → Result**

This is more than adding a chatbot to the platform. ServiceNow is increasingly positioning itself as an **AI platform for enterprise work**, where AI can understand intent, access enterprise context, use tools, execute workflows, collaborate with other agents, and involve humans when necessary.

The key concepts to understand are:

* Now Assist
* Generative AI
* Now LLM Service
* AI Agents
* AI Agent Studio
* Agentic Workflows
* AI Agent Orchestrator
* Otto
* AI Search
* Knowledge and grounding
* Generative AI Controller
* Knowledge Graph
* Model Context Protocol (MCP)
* AI Agent Fabric
* AI Control Tower
* AI governance and security

This article explains how these pieces fit together.

---

# 1. Now Assist

**Now Assist** is ServiceNow's family of generative AI capabilities.

It brings generative AI into ServiceNow applications and workflows, helping users perform tasks such as:

* Summarizing incidents
* Generating resolution notes
* Summarizing conversations
* Generating content
* Suggesting responses
* Finding relevant knowledge
* Assisting developers
* Supporting incident investigation
* Helping employees complete tasks
* Automating portions of existing processes

The important point is that Now Assist is not simply a generic LLM interface.

A standalone LLM might answer:

> "How should I troubleshoot a P1 incident?"

Now Assist can work with the context available in ServiceNow, such as:

* Incidents
* Knowledge articles
* CMDB data
* Users
* Configuration Items
* Workflows
* Policies
* Service Catalog
* Historical records

This leads to an important distinction:

**Traditional GenAI:**
"Give me an answer."

**Now Assist:**
"Help me perform work inside the enterprise process."

---

# 2. The Evolution from Generative AI to Agentic AI

The evolution can be understood in three stages.

## Stage 1 — Generative AI

The AI generates content.

Example:

> "Summarize this incident."

The system produces a summary.

---

## Stage 2 — AI Agents

The AI can reason about a goal and use tools.

Example:

> "Investigate this incident."

The agent might:

1. Read the incident.
2. Check the affected CI.
3. Search related incidents.
4. Search the Knowledge Base.
5. Analyze historical information.
6. Identify potential causes.
7. Produce a recommendation.

The AI is no longer simply generating text.

It is **performing a task**.

---

## Stage 3 — Agentic AI

The AI can plan and execute a multi-step task.

Example:

> "Resolve this incident."

The agent may:

1. Investigate the incident.
2. Identify the affected service.
3. Search for similar incidents.
4. Determine a likely cause.
5. Execute an approved remediation.
6. Validate the result.
7. Update the incident.
8. Escalate if the remediation fails.
9. Involve a human when approval is required.
10. Close the process when appropriate.

This is the fundamental transition:

**From generating answers → to completing work.**

---

# 3. AI Agents

An **AI Agent** can be thought of as a digital worker with:

**Goal + Instructions + Context + Tools + Decision-making capability**

For example, consider an **Incident Triage Agent**.

Its objective might be:

> Analyze incoming incidents and determine their priority, category, impact, and assignment group.

It could have access to tools such as:

* Get Incident
* Query CMDB
* Search Knowledge
* Find Related Incidents
* Get User Information
* Update Incident
* Create Problem
* Trigger Workflow

The agent determines which tools to use and in what sequence to accomplish its objective.

This is very different from a static automation.

A traditional flow might say:

> If priority = P1, assign to Group A.

An agent can potentially reason:

> This incident appears to affect a critical business service. The affected CI has multiple recent incidents. Knowledge article X describes a similar failure. The appropriate assignment group is Y.

The distinction is essentially:

**Automation follows predefined logic.**

**Agents can reason within predefined boundaries and use available tools.**

---

# 4. AI Agent Studio

**AI Agent Studio** is the environment for building, configuring, testing, and managing AI agents.

Conceptually, it sits somewhere between:

* Prompt engineering
* Workflow design
* Tool configuration
* LLM integration
* Automation
* Agent management

A simplified agent configuration might look like:

### Role

> You are an ITSM specialist responsible for investigating critical incidents.

### Instructions

> Analyze the incident, identify affected services, review related incidents, search the Knowledge Base, and provide a recommended next action.

### Tools

* Retrieve Incident
* Search CMDB
* Search Knowledge
* Find Related Incidents
* Update Incident
* Create Problem
* Trigger Change

The agent uses these capabilities to accomplish its goal.

This introduces a new design discipline for ServiceNow developers:

Instead of asking only:

> "What workflow should I build?"

You also ask:

> "What should the agent be allowed to decide, what tools should it have, and where should human approval be required?"

---

# 5. Agentic Workflows

An **AI Agent** is a digital worker.

An **Agentic Workflow** is a process that coordinates one or more digital workers.

For example:

```text
User Request
     |
     v
Agentic Workflow
     |
     +---- Incident Agent
     |
     +---- CMDB Agent
     |
     +---- Knowledge Agent
     |
     +---- Problem Management Agent
     |
     +---- Change Agent
     |
     v
Final Outcome
```

Imagine a user says:

> "Investigate this recurring production issue and determine whether we need a change."

The workflow could coordinate several agents:

1. An Incident Agent analyzes the incidents.
2. A CMDB Agent identifies affected configuration items.
3. A Knowledge Agent searches relevant documentation.
4. A Problem Management Agent determines whether the issue represents a recurring problem.
5. A Change Agent determines whether remediation requires a change.
6. A human may approve the change.
7. ServiceNow executes the appropriate workflow.

This is where ServiceNow's traditional workflow capabilities and new AI capabilities begin to converge.

---

# 6. AI Agent Orchestrator

Once an organization has multiple agents, another problem appears:

**Who decides which agent should act next?**

This is where orchestration becomes important.

An **AI Agent Orchestrator** can coordinate agents and determine how they collaborate to achieve a larger objective.

Conceptually:

```text
                     User
                       |
                       v
              AI Agent Orchestrator
                       |
          +------------+------------+
          |            |            |
          v            v            v
     Incident       CMDB       Knowledge
      Agent         Agent        Agent
          |            |            |
          +------------+------------+
                       |
                       v
                 Change Agent
                       |
                       v
                 Human Approval
                       |
                       v
                    Result
```

The orchestrator acts like a conductor.

Individual agents are specialists.

The orchestrator coordinates them.

---

# 7. Otto

This is where the ServiceNow strategy becomes particularly interesting.

**Otto** represents ServiceNow's move toward a unified AI experience for enterprise work.

ServiceNow has positioned Otto as a broader AI experience that brings together capabilities associated with **Now Assist, Moveworks, and AI Experience**, moving beyond individual AI features toward a more unified, agentic interface.

The important conceptual shift is:

**Now Assist helps users with work.**

**Otto is intended to help users delegate work.**

For example, instead of asking:

> "How do I onboard an employee?"

a user might say:

> "Onboard this employee."

An agentic system could potentially:

* Understand the request.
* Determine which process applies.
* Gather the required information.
* Check permissions.
* Trigger workflows.
* Interact with other systems.
* Request approvals.
* Coordinate multiple agents.
* Track progress.
* Report the outcome.

The interface becomes less about navigating applications and more about expressing **intent**.

---

# 8. Otto as an Enterprise AI Experience

A useful way to think about Otto is:

**Otto = AI experience + enterprise context + agents + orchestration + execution**

Instead of users having to understand exactly where a particular function lives in ServiceNow, they can communicate what they want to accomplish.

For example:

> "I need access to the production environment."

The system could determine:

* Who the user is.
* What access they currently have.
* Which application is involved.
* What policy applies.
* Whether approval is required.
* Which catalog item or workflow should be triggered.
* Which teams need to participate.

The user expresses the **intent**.

The platform handles much of the **execution path**.

---

# 9. LLMs and the Now LLM Service

ServiceNow itself is not an LLM.

LLMs provide language understanding and generation capabilities, while ServiceNow provides the enterprise platform around them.

A simplified architecture looks like:

```text
                LLM
                 |
        Language / Reasoning
                 |
                 v
          ServiceNow AI
                 |
      +----------+----------+
      |          |          |
    Agents     Skills    Workflows
      |          |          |
      +----------+----------+
                 |
                 v
       Enterprise Context
                 |
                 v
        Action / Execution
```

ServiceNow provides capabilities for working with its own models and, depending on the product and configuration, integrating with external model providers.

This distinction is crucial.

The LLM is not the entire solution.

The enterprise AI platform needs to provide:

* Context
* Identity
* Permissions
* Data access
* Tools
* Workflows
* Governance
* Auditing
* Security
* Execution

This is one of ServiceNow's strongest strategic positions.

---

# 10. Grounding and Enterprise Context

One of the biggest risks of using LLMs in enterprises is hallucination.

Suppose an employee asks:

> "What is our VPN access policy?"

A general-purpose LLM does not inherently know the organization's internal policy.

ServiceNow can potentially ground the response using enterprise information such as:

* Knowledge articles
* Policies
* Records
* CMDB
* User information
* Catalog items
* Historical data
* Workflows
* Permissions

This is the concept of **grounding**.

Instead of allowing the model to answer purely from its general knowledge, the system provides relevant enterprise context.

Conceptually:

```text
User Question
      |
      v
Retrieve Relevant Context
      |
      +---- Knowledge
      +---- CMDB
      +---- Records
      +---- Policies
      +---- User Context
      |
      v
      LLM
      |
      v
Grounded Response
```

Grounding is one of the most important concepts when designing enterprise AI systems.

---

# 11. AI Search and Knowledge

**AI Search** plays an important role in making enterprise information discoverable.

Traditional search often depends heavily on keywords.

AI-powered search can understand intent and semantic relationships.

For example:

> "How can I get access to SAP?"

The system can potentially understand that the user is looking for:

* An access request process
* A specific application
* A catalog item
* An approval process
* A relevant Knowledge article

This becomes particularly valuable when combined with agents.

An agent can search for knowledge, consume the result, reason over it, and use the information as part of a larger task.

This creates a powerful chain:

**Search → Context → Reasoning → Action**

---

# 12. Generative AI Controller

The **Generative AI Controller** provides capabilities for managing generative AI integrations and experiences within ServiceNow.

Conceptually, it can serve as a layer between ServiceNow capabilities and the underlying model providers.

A simplified view:

```text
                ServiceNow
                    |
        Generative AI Controller
                    |
          +---------+---------+
          |                   |
       Now LLM          External Models
```

This type of abstraction is important in enterprise environments because organizations typically want centralized governance instead of every application independently connecting to an LLM provider.

---

# 13. Knowledge Graph and Data Context

ServiceNow has another major advantage: its data is highly relational.

Consider a simple ITSM scenario:

```text
Incident
   |
   v
Configuration Item
   |
   v
Application
   |
   v
Business Service
   |
   v
Business Owner
   |
   +---- Change
   |
   +---- Problem
```

A flat document does not naturally represent all these relationships.

ServiceNow's data model does.

This is where concepts such as **Knowledge Graph** become important.

The more effectively an AI system can understand relationships between enterprise entities, the better it can reason about enterprise work.

The AI is no longer just reading text.

It can reason over a connected enterprise context.

---

# 14. MCP — Model Context Protocol

Another important concept in the modern AI ecosystem is **MCP, or Model Context Protocol**.

MCP provides a standardized way for AI systems and agents to interact with external tools and sources of context.

In the ServiceNow ecosystem, this opens an interesting possibility:

```text
External AI / Agent
        |
       MCP
        |
        v
    ServiceNow
        |
   +----+----+----+
   |    |    |    |
 ITSM CMDB Catalog
```

This means ServiceNow can become more than an application that humans interact with.

It can also become a **system of action for AI agents**.

An external agent can potentially interact with ServiceNow capabilities through standardized interfaces instead of requiring every integration to be custom-built from scratch.

---

# 15. AI Agent Fabric

As enterprise AI matures, organizations will probably not have only ServiceNow agents.

They may have:

* ServiceNow agents
* Microsoft agents
* Internal enterprise agents
* Partner agents
* Specialized third-party agents
* Application-specific agents

This creates another challenge:

**How do all these agents communicate and collaborate?**

ServiceNow's **AI Agent Fabric** is positioned around this problem.

The broader idea is to connect agents and tools across different environments.

Conceptually:

```text
                  Enterprise
                      |
              AI Agent Fabric
                      |
       +--------------+--------------+
       |              |              |
 ServiceNow       Microsoft       Custom
   Agents          Agents         Agents
       |              |              |
       +--------------+--------------+
                      |
               Enterprise Work
```

The long-term vision is therefore not necessarily:

> "Everything must be a ServiceNow agent."

It is closer to:

> "ServiceNow can become a trusted orchestration and execution layer for enterprise AI."

---

# 16. AI Control Tower

Once an organization has hundreds or thousands of AI agents, governance becomes a major concern.

Organizations need to know:

* Which agents exist?
* Who owns them?
* What can they do?
* Which data can they access?
* Which tools can they invoke?
* How often are they used?
* What actions are they taking?
* Are they performing correctly?
* What happens when something goes wrong?

This is where concepts such as **AI Control Tower** become important.

The control layer provides centralized visibility and governance over the organization's AI ecosystem.

The problem is no longer simply:

> "How do we build an AI agent?"

It becomes:

> "How do we safely operate 5,000 AI agents?"

That is an enterprise governance problem.

---

# 17. Security and Governance

Agentic AI introduces a completely different risk profile from traditional generative AI.

A chatbot that produces a wrong answer is one problem.

An agent that performs the wrong action is a much bigger problem.

For example:

```text
LLM
 |
 v
Agent
 |
 v
Tool
 |
 v
Update Record
 |
 v
Production Impact
```

This means agent design must consider:

* Identity
* Role-based access
* Data permissions
* Tool permissions
* Human approvals
* Guardrails
* Auditability
* Logging
* Observability
* Error handling
* Escalation
* Scope limitations

A good enterprise agent should not simply be:

> "An LLM with access to everything."

It should be:

> "An AI worker with a clearly defined role, bounded permissions, approved tools, observable actions, and controlled escalation paths."

---

# 18. ServiceNow's AI Architecture — Putting Everything Together

A simplified conceptual architecture could look like this:

```text
                         USER
                           |
                           v
                         OTTO
                           |
                           v
                  AI EXPERIENCE LAYER
                           |
                           v
                 AGENT ORCHESTRATION
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
      AI Agent         AI Agent         AI Agent
       ITSM              HR              CSM
          |                |                |
          +----------------+----------------+
                           |
                           v
                     AI WORKFLOWS
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
       Tools           Workflows         Skills
          |                |                |
          +----------------+----------------+
                           |
                           v
                 ENTERPRISE CONTEXT
                           |
        +------------------+------------------+
        |                  |                  |
        v                  v                  v
      CMDB             Knowledge           Records
        |                  |                  |
        +------------------+------------------+
                           |
                           v
                     LLM / MODELS
                           |
                           v
                    ACTION / RESULT
```

Around all of this sit:

* Security
* Governance
* Identity
* Observability
* AI Control
* Integration
* Data protection

---

# 19. The Most Important Conceptual Shift

The most important change is not actually the introduction of an LLM.

It is the shift from **application-centric interaction** to **intent-centric interaction**.

Traditional ServiceNow:

> "I need to open an incident."

The user navigates to the appropriate application, finds the correct form, fills it out, submits it, and follows the process.

AI-driven ServiceNow:

> "My production application is down."

The system can potentially determine:

* This is an incident.
* The application is business-critical.
* The user is associated with a particular service.
* The affected CI can be identified.
* Similar incidents exist.
* A known Knowledge article may apply.
* A particular assignment group should handle it.
* A specific workflow should be triggered.

The user expresses **intent**.

The platform interprets and executes the **work**.

---

# 20. What This Means for ServiceNow Developers

This evolution changes the skill set required from ServiceNow professionals.

Traditional ServiceNow development focuses heavily on:

* Tables
* Forms
* Business Rules
* Client Scripts
* Script Includes
* Flow Designer
* IntegrationHub
* APIs
* ACLs
* Workflows

These skills remain important.

But AI introduces another layer:

* Prompt engineering
* Agent design
* Tool design
* Context engineering
* Grounding
* RAG concepts
* Agent orchestration
* AI governance
* AI security
* Human-in-the-loop design
* Agent evaluation
* Observability
* AI lifecycle management

The developer increasingly needs to ask:

> What should be deterministic?

> What should be handled by AI?

> What should the agent be allowed to decide?

> What should remain a traditional workflow?

> What tools should the agent have?

> What data should the agent access?

> When should a human approve the action?

> How do we audit the decision?

This is a fundamental change in platform engineering.

---

# 21. A Useful Mental Model

One of the easiest ways to remember the ecosystem is:

### Now Assist

**AI assistance inside ServiceNow.**

### AI Agents

**Digital workers capable of accomplishing specific goals.**

### AI Agent Studio

**Where agents are designed and managed.**

### Agentic Workflows

**Processes composed of AI-driven tasks and agents.**

### AI Agent Orchestrator

**Coordinates multiple agents and their activities.**

### Otto

**A unified AI experience through which users can interact with and delegate work to the ServiceNow AI ecosystem.**

### LLMs

**The language and reasoning engines behind many AI capabilities.**

### AI Search / Knowledge

**Provides relevant enterprise context.**

### Knowledge Graph / Enterprise Data

**Provides relationships and structure between enterprise entities.**

### MCP

**A standardized way for AI systems to interact with tools and context.**

### AI Agent Fabric

**Connects agents across platforms and ecosystems.**

### AI Control Tower

**Provides visibility, governance, and control over the enterprise AI ecosystem.**

---

# 22. The Big Picture

The evolution can be summarized as:

```text
Automation
    ↓
Workflow
    ↓
Generative AI
    ↓
AI Assistance
    ↓
AI Agents
    ↓
Agentic Workflows
    ↓
Multi-Agent Orchestration
    ↓
Enterprise AI Ecosystem
    ↓
AI-Native Enterprise
```

ServiceNow's strategic opportunity is to sit at the intersection of:

**Data + AI + Workflow + Enterprise Applications + Integration + Governance**

That combination is powerful.

A general-purpose LLM may be excellent at understanding language and reasoning.

ServiceNow knows how enterprise work is structured.

It knows about:

* Employees
* Incidents
* Problems
* Changes
* Services
* Configuration Items
* Approvals
* Policies
* Catalogs
* Workflows
* Business processes

The real value emerges when these capabilities are combined.

---

# Conclusion

ServiceNow's AI strategy is moving beyond "adding AI features" to enterprise applications.

The larger vision is an **AI-native enterprise workflow platform** where users express intent and AI agents perform work across systems.

The progression can be summarized as:

**Now Assist → AI Agents → Agentic Workflows → Orchestration → Otto → Enterprise Agent Ecosystem**

The most important idea to remember is:

> **The future of ServiceNow is not simply AI that talks about work. It is AI that understands, coordinates, and performs work.**

For ServiceNow professionals, this means the platform is evolving from a system where developers primarily define deterministic processes into a platform where developers also design **AI workers, their tools, their context, their boundaries, and the way they collaborate with humans and other agents**.

That is the real significance of the move from Now Assist toward Otto and the broader ServiceNow AI platform.
