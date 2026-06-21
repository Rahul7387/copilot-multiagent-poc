# Architecture deep dive

## Overview

This POC uses **connected agents** — independently published Copilot Studio agents that the orchestrator calls at runtime.

## Orchestration type: Generative

The orchestrator uses generative orchestration, which means:
- No hardcoded routing rules or if/else conditions
- The AI reads each specialist agent's description to decide who to call
- Routing improves as descriptions become more specific and distinct

## Data flow

```
1. Employee sends message
2. Orchestrator receives message
3. AI reads intent + all connected agent descriptions
4. Orchestrator calls the most relevant specialist agent
5. Specialist agent processes the query using its own topics/knowledge
6. Response flows back through the orchestrator to the employee
7. Orchestrator asks if the employee needs further help
```

## Context passing

By default, Copilot Studio passes the full conversation history to the specialist agent. This means:
- The specialist knows what was already discussed
- No need to re-ask for information already collected
- For user-specific variables (employee ID, name), pass them explicitly

## Connected vs child agents

| Feature | Connected Agent | Child Agent |
|---|---|---|
| Lives inside parent | No — standalone | Yes — embedded |
| Reusable across agents | Yes | No |
| Needs publishing | Yes | No |
| Own settings/auth | Yes | No |
| Best for | Separate teams, reuse | Single team, quick POC |

This POC uses **connected agents** for modularity and independent team ownership.
