# Troubleshooting guide

## Issue: Specialist agent not showing in the connect list

Cause: The specialist agent is not published or sharing is not enabled.

Fix:
1. Open the specialist agent in Copilot Studio
2. Click Publish (top right)
3. Go to Settings → Security
4. Enable "Share with other agents in this environment"
5. Return to the orchestrator and try connecting again

---

## Issue: Orchestrator routes to the wrong agent

Cause: Agent descriptions are too vague or overlap with each other.

Fix:
- Make each description unique and domain-specific
- List the exact topics each agent covers
- Avoid generic phrases like "answers employee questions" — be specific
- Add the words the user is likely to say in the description

---

## Issue: Activity Map shows no agent delegation

Cause: The orchestrator is answering directly instead of delegating.

Fix:
- Update the orchestrator's instructions to explicitly say "never answer specialist questions yourself — always delegate"
- Ensure the connected agents are visible in the Agents tab of the orchestrator
- Test with a clear domain-specific query

---

## Issue: Cross-domain queries only answered by one agent

Cause: The orchestrator stopped after the first delegation.

Fix:
- Add to orchestrator instructions: "If a query spans multiple domains, address them one at a time by calling each relevant agent in sequence"
- Test with: "I'm a new joiner — what IT access and HR documents do I need?"

---

## Issue: Conversation history not available in specialist agent

Cause: History passing is disabled.

Fix:
- In the orchestrator, click on the connected agent
- Ensure "Pass conversation history" is turned on

---

## Issue: Agent description not updated after edit

Fix: Republish the specialist agent after any description change. The orchestrator reads the description at runtime from the published version.
