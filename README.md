# ServiceNow AI Agent — Change Risk Analyzer

> Built on ServiceNow Zurich | AI Agent Studio | Now Assist | Agentic Workflows

---

## Overview

An AI-powered ServiceNow agent that automatically analyzes Change Requests and generates a complete planning package — including implementation plan, backout plan, test plan, and risk assessment — using historical change data and Knowledge Base articles as grounding sources.

Built end-to-end on a Personal Development Instance (PDI) running ServiceNow Zurich as part of the NowForge Episode 1 hands-on lab.

---

## The Problem

When a Change Request is submitted in ServiceNow, a Change Manager typically spends **30–60 minutes** manually:

* Reading and understanding the change description
* Recalling similar past changes from memory
* Searching the Knowledge Base for documented procedures
* Writing an implementation plan
* Writing a backout plan
* Writing a test plan
* Assigning a risk score with justification

This agent reduces that to **under 2 minutes**, with a human approval gate before anything is saved.

---

## Architecture

```
User (Now Assist Panel)
        │
        ▼
Agentic Workflow — Change Risk Analysis and Planning
  (Orchestrator)
        │
        ▼
AI Agent — Change Management Specialist
        │
        ├── Tool 0: Get Change Details        [Autonomous — Read]
        ├── Tool 1: Find Similar Past Changes [Autonomous — Read]
        ├── Tool 2: Search Change KB          [Autonomous — Read]
        └── Tool 3: Update Change with Plans  [Supervised — Write]
                        │
                        ▼
              Human Approval Gate
                        │
                        ▼
              Change Record Updated
```

---

## How It Works

1. User opens the **Now Assist Panel** and types:
   `Analyze the change request CHG0030010 and generate the complete planning package.`

2. The **Agentic Workflow** receives the message and delegates to the AI Agent.

3. The AI Agent executes a 6-step process:

   * **Step 1:** Fetch change details
   * **Step 2:** Retrieve similar historical changes
   * **Step 3:** Search Knowledge Base
   * **Step 4:** Generate planning package
   * **Step 5:** Pause for approval before writing
   * **Step 6:** Output final summary

4. The **Change Manager reviews and approves**

5. The record is updated automatically

---

## Tools

### Tool 0 — Get Change Details

* Fetches full Change Request record
* Returns: sys_id, description, type, category, state
* Execution: Autonomous (read-only)

### Tool 1 — Find Similar Past Changes

* Searches historical Change Requests
* Returns: up to 5 relevant past changes
* Execution: Autonomous (read-only)

### Tool 2 — Search Change KB

* Searches Knowledge Base articles
* Returns: up to 3 relevant documents
* Execution: Autonomous (read-only)

### Tool 3 — Update Change with Plans

* Writes generated content to record
* Execution: **Supervised (requires approval)**

---

## Key Design Decisions

| Decision                       | Reason                                                 |
| ------------------------------ | ------------------------------------------------------ |
| Supervised write tool          | Prevents incorrect AI output from saving automatically |
| Read-before-write architecture | Grounds AI in real data                                |
| Anti-hallucination rules       | Prevents fake change data                              |
| User-based access control      | Maintains security integrity                           |
| Output truncation              | Keeps within LLM limits                                |
| State validation               | Prevents editing closed/invalid records                |

---

## Tech Stack

| Component     | Technology              |
| ------------- | ----------------------- |
| Platform      | ServiceNow Zurich       |
| AI Interface  | Now Assist              |
| Orchestration | Agentic Workflows       |
| Agent         | AI Agent Studio         |
| Scripting     | JavaScript (Glide APIs) |
| Data Access   | GlideRecord             |
| AI Engine     | Now LLM Service         |

---

## Sample Output

**Implementation Plan (excerpt):**

```
1. Notify dependent teams
2. Verify backup
3. Apply patch to replica
4. Validate system
5. Failover and patch primary
6. Restore traffic
```

**Risk Score:** Moderate

**Risk Reasoning:**
Production impact with rollback capability; controlled maintenance window reduces risk.

---

## Verification

System logs confirm execution:

```
[NF-Tool0] fetched CHG0030010
[NF-Tool1] keywords=database patch results=1
[NF-Tool2] query=database patching results=1
[NF-Tool3] updated CHG0030010
```

---

## Author

Parva Ghanbarian

---

## Certifications 

* ServiceNow Certified System Administrator (CSA)
* ServiceNow Certified Application Developer (CAD)

---
