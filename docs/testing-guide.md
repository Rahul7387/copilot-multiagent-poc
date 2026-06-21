# Testing guide

## Phase 1: Test each agent individually

Before connecting agents, test each specialist agent in isolation.

### HR Policy Agent
Open HR Policy Agent → Test your agent

Test queries:
- "How many days of annual leave do I get?"
- "When is my salary credited this month?"
- "I want to apply for maternity leave"
- "What health insurance benefits are available?"

Expected: Each query triggers the correct topic and returns a relevant answer.

---

### IT Helpdesk Agent
Open IT Helpdesk Agent → Test your agent

Test queries:
- "I forgot my password and can't log in"
- "How do I connect to the VPN from home?"
- "I need access to Adobe Creative Cloud"
- "My laptop screen is broken"

Expected: Step-by-step guidance or ticket creation for each.

---

### Learning & Training Agent
Open Learning & Training Agent → Test your agent

Test queries:
- "What courses are available for project management?"
- "Is my compliance training due this week?"
- "I want to get Azure certified — where do I start?"

Expected: Course recommendations, deadline alerts, or learning path suggestions.

---

## Phase 2: Test orchestrator routing

Open Employee Support Bot → Test your agent

### Single-domain routing tests

| Query | Expected agent |
|---|---|
| "How many days leave do I get?" | HR Policy Agent |
| "I forgot my password" | IT Helpdesk Agent |
| "What courses are available?" | Learning & Training Agent |
| "When is my salary paid?" | HR Policy Agent |
| "How do I set up VPN?" | IT Helpdesk Agent |
| "Is my compliance training due?" | Learning & Training Agent |

### Cross-domain routing tests

Query: "I'm joining next week — what IT access do I need and what HR forms to fill?"
Expected: Orchestrator calls both HR and IT agents in sequence

Query: "I completed my Azure certification — does HR know about this?"
Expected: Orchestrator calls Training agent, then potentially HR agent

---

## Phase 3: Verify in Activity Map

For every test query:
1. Send the message in the test panel
2. Click the Activity Map icon (top of test panel)
3. Confirm the chain shows: Orchestrator → correct specialist agent
4. Check no unnecessary agents were called

---

## Phase 4: Edge case tests

- "Hello" → Orchestrator greets and asks how to help (no routing)
- "What's the weather today?" → Orchestrator should gracefully say it's out of scope
- "I need help" → Orchestrator should ask a clarifying question
- Very long message spanning all three domains → Orchestrator should handle sequentially
