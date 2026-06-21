# 🤖 Copilot Studio Multi-Agent POC — Employee Support Bot

[![Microsoft Copilot Studio](https://img.shields.io/badge/Microsoft-Copilot%20Studio-0078D4?style=flat&logo=microsoft)](https://copilotstudio.microsoft.com)
[![Power Platform](https://img.shields.io/badge/Power-Platform-742774?style=flat&logo=microsoft)](https://powerplatform.microsoft.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> A fully documented, step-by-step Proof of Concept (POC) for building a **Multi-Agent System** in Microsoft Copilot Studio. One orchestrator agent intelligently routes employee queries to three specialist agents — HR, IT, and Training — with zero manual routing by the user.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Agents](#-agents)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Step-by-Step Setup](#-step-by-step-setup)
- [Agent Instructions](#-agent-instructions)
- [Topics Reference](#-topics-reference)
- [Testing Guide](#-testing-guide)
- [Folder Structure](#-folder-structure)
- [Best Practices](#-best-practices)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

Traditional chatbots struggle when a single user query spans multiple domains — HR policy, IT support, and learning management all at once. This POC solves that with **multi-agent orchestration**:

- The user sends **one message** to one bot
- The **orchestrator agent** reads intent and delegates to the right specialist
- Specialist agents handle their domain with **dedicated knowledge and topics**
- The response flows back **seamlessly** to the user

### Why this POC?

| Without Multi-Agent | With Multi-Agent |
|---|---|
| One massive agent with 50+ topics | Modular, focused agents (5–10 topics each) |
| Degraded routing above 30–40 actions | Clean separation of concerns |
| Single team manages everything | Teams own their agent independently |
| Hard to test and debug | Activity Map shows exact orchestration chain |

---

## 🏗️ Architecture

```
Employee (User)
      │
      ▼
┌─────────────────────────────┐
│   🎯 Employee Support Bot   │  ← Main Orchestrator Agent
│   (Parent / Orchestrator)   │
└───────────┬─────────────────┘
            │  Routes by intent
   ┌─────────┼─────────┐
   ▼         ▼         ▼
┌──────┐ ┌──────┐ ┌──────────┐
│  👥  │ │  💻  │ │    📚    │
│  HR  │ │  IT  │ │ Training │
│Agent │ │Agent │ │  Agent   │
└──────┘ └──────┘ └──────────┘
```

**Orchestration type:** Connected Agents (independently published, reusable)

**Routing mechanism:** Generative orchestration — the parent agent reads each specialist's **description** and uses AI to decide which agent to call.

---

## 🤖 Agents

### 🎯 Employee Support Bot (Orchestrator)
- **Role:** Central entry point. Routes all queries.
- **Type:** Parent / Main agent
- **Does NOT** answer specialist questions directly
- Maintains conversation context across agent calls

### 👥 HR Policy Agent
- **Role:** All human resources queries
- **Topics:** Leave policy, payroll FAQ, benefits, onboarding, offboarding
- **Type:** Connected agent (published independently)

### 💻 IT Helpdesk Agent
- **Role:** All IT support requests
- **Topics:** Password reset, VPN setup, software access, raise IT ticket, hardware issues
- **Type:** Connected agent (published independently)

### 📚 Learning & Training Agent
- **Role:** All learning and development queries
- **Topics:** Course catalogue, certification tracker, compliance deadlines, learning paths
- **Type:** Connected agent (published independently)

---

## ✅ Prerequisites

- Access to [Microsoft Copilot Studio](https://copilotstudio.microsoft.com)
- A **Power Platform environment** (Developer or Production)
- Microsoft 365 license with Copilot Studio entitlement
- Basic understanding of Copilot Studio topics and agents

---

## ⚡ Quick Start

```
1. Create HR Policy Agent         → Publish → Enable sharing
2. Create IT Helpdesk Agent       → Publish → Enable sharing
3. Create Learning & Training Agent → Publish → Enable sharing
4. Create Employee Support Bot (Orchestrator)
5. Connect all 3 specialist agents (Agents tab → Add)
6. Add instructions to Orchestrator
7. Test with the Activity Map
```

> **Important:** All specialist agents must be **published** and **sharing enabled** before the orchestrator can connect to them.

---

## 📖 Step-by-Step Setup

### Step 1 — Create the HR Policy Agent

1. Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Left nav → **Agents** → **New Agent**
3. Name: `HR Policy Agent`
4. Set description *(critical for routing!)*:
   ```
   Handles employee questions about HR policies: leave, payroll,
   benefits, onboarding, and offboarding procedures.
   ```
5. Add topics (see [Topics Reference](#-topics-reference))
6. **Publish** the agent
7. Settings → Security → Enable **"Share with other agents in this environment"**

### Step 2 — Create the IT Helpdesk Agent

1. Create new agent
2. Name: `IT Helpdesk Agent`
3. Description:
   ```
   Resolves IT support requests: password resets, VPN access,
   software installation, hardware issues, and account lockouts.
   ```
4. Add IT topics
5. Publish and enable sharing

### Step 3 — Create the Learning & Training Agent

1. Create new agent
2. Name: `Learning & Training Agent`
3. Description:
   ```
   Guides employees on available training courses, certifications,
   learning paths, and mandatory compliance training deadlines.
   ```
4. Add training topics
5. Publish and enable sharing

### Step 4 — Create the Orchestrator

1. Create new agent
2. Name: `Employee Support Bot`
3. Description:
   ```
   Central assistant for all employee queries. Routes HR, IT,
   and training questions to the right specialist agent.
   ```
4. Paste the orchestrator instructions (see [Agent Instructions](#-agent-instructions))
5. Go to **Agents tab** → **Add** → **Connect to a Copilot Studio agent**
6. Connect all three specialist agents
7. Test before publishing

---

## 📝 Agent Instructions

### Orchestrator Instructions

```
You are the Employee Support Bot, the central assistant for all employee queries.

Your job is to understand what the employee needs and delegate to the right
specialist agent:
- HR questions (leave, payroll, benefits, onboarding) → HR Policy Agent
- IT issues (passwords, VPN, software, hardware) → IT Helpdesk Agent
- Learning/training (courses, certifications, compliance) → Learning & Training Agent

Always greet the employee warmly. If a query spans multiple domains, address
them one at a time. Do not attempt to answer specialist questions yourself —
always delegate to the appropriate agent.

If you are unsure which agent to route to, ask the employee a clarifying
question to better understand their need.
```

See full instructions for each agent in the [`/agents`](./agents) folder.

---

## 📂 Topics Reference

### HR Policy Agent Topics

| Topic Name | Trigger Phrases |
|---|---|
| Leave Policy | "how many days leave", "apply for annual leave", "maternity policy", "sick leave rules" |
| Payroll FAQ | "when is salary credited", "payslip download", "salary revision", "pay date" |
| Benefits & Insurance | "health insurance", "medical cover", "employee benefits", "provident fund" |
| Onboarding Help | "joining formalities", "first day checklist", "new employee documents" |

### IT Helpdesk Agent Topics

| Topic Name | Trigger Phrases |
|---|---|
| Password Reset | "forgot password", "account locked", "reset my password", "can't log in" |
| VPN Setup | "connect to VPN", "remote access", "VPN not working", "office network from home" |
| Software Access | "install software", "request app access", "need license for", "software request" |
| Raise IT Ticket | "log a ticket", "raise IT issue", "create support request" |
| Hardware Issues | "laptop not working", "request new hardware", "monitor issue", "keyboard broken" |

### Learning & Training Agent Topics

| Topic Name | Trigger Phrases |
|---|---|
| Course Catalogue | "available courses", "what training", "list of programs", "learning options" |
| Certification Tracker | "my certifications", "certification status", "certificate expired", "badge progress" |
| Compliance Deadlines | "compliance training due", "mandatory training", "training deadline", "is my training complete" |
| Learning Paths | "recommended courses", "career learning path", "skills development", "upskilling" |

---

## 🧪 Testing Guide

### Test in the Copilot Studio Test Panel

Open **Employee Support Bot** → click **Test your agent** (right panel).

### Recommended Test Queries

```
HR Domain:
"How many days of annual leave do I get?"
"When is my salary credited this month?"
"What health insurance benefits do I have?"

IT Domain:
"I forgot my password and can't log in"
"How do I connect to the VPN from home?"
"I need access to the design software"

Training Domain:
"What courses are available for project management?"
"Is my compliance training due this week?"
"Show me learning paths for data analysis"

Cross-Domain (Advanced):
"I'm a new joiner — what IT access do I need and what HR forms to fill?"
```

### Verifying Correct Routing

1. Send a test message
2. Click the **Activity Map** icon in the test panel
3. Verify the chain: `Main Agent → delegates to → correct specialist agent`

### What Good Routing Looks Like

```
User: "I forgot my password"
  └─> Employee Support Bot (Orchestrator)
        └─> IT Helpdesk Agent
              └─> Password Reset topic
                    └─> Response to user ✅
```

---

## 📁 Folder Structure

```
copilot-multiagent-poc/
│
├── README.md                          # This file
├── CONTRIBUTING.md                    # Contribution guidelines
├── LICENSE                            # MIT License
│
├── agents/
│   ├── orchestrator/
│   │   ├── instructions.md            # Orchestrator system instructions
│   │   └── description.md            # Agent description for routing
│   ├── hr-agent/
│   │   ├── instructions.md
│   │   ├── description.md
│   │   └── topics.md                  # All HR topics + trigger phrases
│   ├── it-agent/
│   │   ├── instructions.md
│   │   ├── description.md
│   │   └── topics.md
│   └── training-agent/
│       ├── instructions.md
│       ├── description.md
│       └── topics.md
│
├── docs/
│   ├── architecture.md                # Architecture deep dive
│   ├── connected-vs-child-agents.md   # When to use each type
│   ├── testing-guide.md               # Full testing scenarios
│   └── troubleshooting.md             # Common issues and fixes
│
└── assets/
    └── architecture-diagram.png       # Visual architecture diagram
```

---

## 💡 Best Practices

### Writing Agent Descriptions (Most Important!)

The orchestrator uses each specialist agent's **description** to decide where to route. Write them carefully:

```
✅ Good:
"Handles employee questions about HR policies: leave, payroll,
 benefits, onboarding, and offboarding procedures."

❌ Bad:
"Answers employee questions"  ← Too vague, causes wrong routing
```

### Routing Performance

- Keep each specialist agent to **5–15 topics** for best performance
- If the orchestrator has **30+ action choices**, consider splitting further
- Always test with **edge-case queries** that span multiple domains

### Context Passing

Copilot Studio automatically passes conversation history when one agent calls another. However:
- For user-specific data (name, employee ID), pass it explicitly as a variable
- Avoid redundant questions — check if the orchestrator already captured the info

### Security

- Each specialist agent should only have access to its own domain's knowledge
- Use **least privilege** — don't give the IT agent access to HR Dataverse tables
- Enable **audit logging** via Dataverse transcripts for compliance

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Ideas for contribution:**
- Add more specialist agents (Finance, Facilities, Legal)
- Add Power Automate flows for ticket creation
- Add Dataverse knowledge sources
- Improve routing instructions
- Add more topic examples with adaptive cards

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🔗 Resources

- [Microsoft Copilot Studio Docs](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Multi-Agent Orchestration Guide](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-add-other-agents)
- [Multi-Agent Patterns & Best Practices](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/multi-agent-patterns)
- [Copilot Studio Labs (Official Microsoft)](https://microsoft.github.io/mcs-labs/)

---

<p align="center">Built with ❤️ using Microsoft Copilot Studio</p>
