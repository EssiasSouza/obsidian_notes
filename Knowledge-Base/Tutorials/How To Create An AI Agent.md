Source: #source/internet_resources 
Project: #project/artificial_intelligence
Areas: #area/work
Subject: #subect/agentic_ai
Type: #type/learning 
Learning priority: #priority/P2 
Status: #status/to_learning
Related: [[AI]]

---

# Tutorial: Building AI Agents from Scratch with ChatGPT

## Introduction

An AI agent is much more than a prompt.

A well-designed agent is composed of:

* Personality
* Mission
* Rules
* Knowledge
* Workflows
* Examples
* Outputs
* Memory (optional)
* Tools (optional)

Think of an AI agent as a professional employee.

A professional electrician doesn't only know electricity.

He also knows:

* Safety procedures
* Applicable regulations
* Diagnostic methodology
* Communication style
* How to create reports
* Which questions to ask
* When to refuse unsafe work

Exactly the same applies to AI Agents.

---

# Step 1 - Create the Project

Create a project directory.

```text
ElectricianAgent/
```

Inside it create the following structure.

```text
ElectricianAgent/
│
├── prompts/
├── knowledge/
├── examples/
├── workflows/
├── output/
├── docs/
├── config/
└── README.md
```

This structure is intentionally simple.

Each folder has one responsibility.

| Folder    | Purpose                 |
| --------- | ----------------------- |
| prompts   | System prompts          |
| knowledge | Technical documentation |
| examples  | Example conversations   |
| workflows | Standard procedures     |
| output    | Generated reports       |
| docs      | Project documentation   |
| config    | Agent configuration     |

---

# Step 2 - Ask ChatGPT to Create the Agent Identity

Instead of writing everything manually, use ChatGPT.

Example prompt:

```text
You are an expert prompt engineer.

Create the system prompt for an AI Electrician Agent.

The agent should:

- Act as a licensed electrician.
- Follow electrical safety best practices.
- Never encourage unsafe work.
- Ask clarifying questions before diagnosing.
- Explain technical concepts in simple language.
- Produce structured reports.
- Suggest preventive maintenance.
- Warn when a licensed professional is required.

Return the result in Markdown.
```

Save the generated file as:

```text
prompts/system_prompt.md
```

---

# Step 3 - Build the Knowledge Base

Now ask ChatGPT to generate the technical knowledge.

Example:

```text
Generate a knowledge base for an AI electrician.

Include:

- Basic electrical concepts
- Voltage
- Current
- Resistance
- Power
- AC vs DC
- Circuit breakers
- Grounding
- Electrical panels
- Residential wiring
- Safety
- Common failures
- Troubleshooting
```

Save as

```text
knowledge/electrical_basics.md
```

---

# Step 4 - Create Specialized Documents

Instead of one giant file, divide the knowledge.

Example:

```text
knowledge/

electrical_basics.md

safety.md

grounding.md

circuit_breakers.md

lighting.md

motors.md

residential_installation.md

commercial_installation.md

inspection_checklist.md
```

Small documents are easier to maintain.

---

# Step 5 - Create Workflows

Agents perform better when they follow a process.

Example prompt:

```text
Create a troubleshooting workflow for diagnosing electrical problems.

Steps:

1. Gather symptoms
2. Identify affected area
3. Check power source
4. Inspect protection devices
5. Isolate the circuit
6. Suggest possible causes
7. Recommend corrective actions
8. Generate a report
```

Save as

```text
workflows/troubleshooting.md
```

---

# Step 6 - Create Examples

Examples teach the model how to behave.

Example:

```text
User:

The kitchen outlet stopped working.

Assistant:

Let's investigate.

Questions:

1. Are other outlets working?
2. Did the circuit breaker trip?
3. Was any appliance recently connected?
4. Do you smell burning?
5. Is there power elsewhere in the house?
```

Save as

```text
examples/outlet_failure.md
```

Create many examples.

Examples are often more valuable than additional instructions.

---

# Step 7 - Create Report Templates

The agent should always produce standardized reports.

Example:

```markdown
# Electrical Inspection Report

## Customer

## Location

## Problem Description

## Observations

## Possible Causes

## Recommended Actions

## Safety Notes

## Next Steps
```

Save as

```text
output/report_template.md
```

---

# Step 8 - Generate More Content with ChatGPT

Continue expanding the project by asking ChatGPT to generate additional materials.

Examples:

```text
Create 30 common residential electrical problems.
```

```text
Generate a preventive maintenance checklist.
```

```text
Generate a decision tree for electrical diagnostics.
```

```text
Generate frequently asked questions from homeowners.
```

```text
Generate electrical terminology.
```

```text
Generate a troubleshooting guide.
```

Each generated document should be stored in the appropriate folder.

---

# Step 9 - Organize the Knowledge

A simple organization could look like this:

```text
knowledge/

electrical_basics.md
safety.md
lighting.md
motors.md
panels.md
grounding.md
inspection.md
maintenance.md
faq.md
glossary.md
```

Avoid creating one document with hundreds of pages.

Smaller documents are easier to update and reuse.

---

# Step 10 - Create the README

Document the project.

Example:

```markdown
# Electrician Agent

Purpose

Assist users in diagnosing residential electrical problems safely.

Main Features

- Troubleshooting
- Report generation
- Preventive maintenance
- Safety recommendations

Knowledge Sources

- Internal documentation
- Safety standards
- Best practices

Limitations

- Does not replace a licensed electrician.
- Never recommends unsafe procedures.
```

---

# Example of a Simple Project Structure

```text
ElectricianAgent/
│
├── prompts/
│   └── system_prompt.md
│
├── knowledge/
│   ├── electrical_basics.md
│   ├── safety.md
│   ├── grounding.md
│   ├── circuit_breakers.md
│   └── lighting.md
│
├── workflows/
│   └── troubleshooting.md
│
├── examples/
│   └── outlet_failure.md
│
├── output/
│   └── report_template.md
│
├── docs/
│   └── architecture.md
│
├── config/
│   └── settings.yaml
│
└── README.md
```

## Tips for Growing Your Agent

As the project evolves, consider adding:

* Multiple personas (for example, residential, industrial, and commercial electricians)
* Versioned prompts
* Retrieval-Augmented Generation (RAG) for searching documentation
* Automated tests for prompts
* Evaluation datasets to measure answer quality
* External tools such as calculators or APIs
* Memory to retain user preferences
* Logging and observability for debugging and performance monitoring

# Example of a More Advanced Agent Structure

Below is an example of how the same project might evolve into a production-ready agent.

```text
ElectricianAgent/
│
├── prompts/
│   ├── system/
│   │   ├── core.md
│   │   ├── safety.md
│   │   ├── communication.md
│   │   └── limitations.md
│   │
│   ├── personas/
│   │   ├── residential.md
│   │   ├── commercial.md
│   │   └── industrial.md
│   │
│   └── templates/
│       ├── inspection.md
│       ├── diagnosis.md
│       └── maintenance.md
│
├── knowledge/
│   ├── fundamentals/
│   ├── residential/
│   ├── commercial/
│   ├── industrial/
│   ├── safety/
│   ├── regulations/
│   ├── troubleshooting/
│   ├── glossary/
│   └── faq/
│
├── workflows/
│   ├── diagnosis/
│   ├── inspection/
│   ├── maintenance/
│   └── emergency/
│
├── examples/
│   ├── conversations/
│   ├── reports/
│   └── edge_cases/
│
├── tools/
│   ├── load_calculator.py
│   ├── voltage_drop.py
│   └── cable_sizing.py
│
├── tests/
│   ├── prompts/
│   ├── workflows/
│   └── evaluations/
│
├── output/
│   ├── reports/
│   ├── inspections/
│   └── logs/
│
├── docs/
│   ├── architecture.md
│   ├── roadmap.md
│   ├── changelog.md
│   └── contributing.md
│
├── config/
│   ├── settings.yaml
│   ├── models.yaml
│   └── permissions.yaml
│
├── scripts/
│   ├── build.py
│   ├── validate.py
│   └── package.py
│
├── assets/
│   ├── diagrams/
│   └── images/
│
└── README.md
```

This more robust structure follows software engineering principles by separating responsibilities, thereby facilitating the agent's maintenance, testing, and evolution. The same pattern can be reused for virtually any domain simply by replacing the contents of the `knowledge` folder, adapting the `workflows`, and adjusting the `system prompt` to reflect the desired specialization.

If you need work with different harness you can follow this.

|Ferramenta|Arquivo recomendado|
|---|---|
|Codex CLI|✅ `AGENTS.md`|
|Claude Code|✅ `CLAUDE.md`|
|Cursor|✅ `.cursor/rules/*.mdc`|
|OpenHands|Configuração via SDK/UI|
|OpenClaw|✅ `AGENTS.md`|
|Antigravity|✅ `GEMINI.md`|
|OpenAI Agents SDK|Prompt definido em código|