# Developer Guide: Integrating AccessCard and TransactionCard into A2A-Compatible Agents

This guide is for developers building agents, gateways, or orchestration systems that interact with the [A2A Protocol](https://github.com/google/A2A)
and wish to extend it using the Opn.li `AccessCard` and `TransactionCard` schema for trusted, permissioned data access.

---

## 🎯 Purpose

Opn.li's CARD model introduces **machine-readable, enforceable trust frameworks** for agent-driven data access and automation.

- **AccessCard** defines *what* an agent is allowed to access.
- **TransactionCard** defines *how* that access is controlled, constrained, and consented.

This builds on the A2A Protocol’s JSON-RPC model by adding a **verifiable layer of authorization** that lives inside the `metadata` of a task.

---

## 🧱 A2A + CARD Interaction Overview

A typical A2A `SendTaskRequest` message extended with CARDs might look like:

```json
{
  "jsonrpc": "2.0",
  "id": "task-001",
  "method": "tasks/send",
  "params": {
    "id": "task-001",
    "sessionId": "session-abc",
    "message": {
      "role": "user",
      "parts": [
        {
          "type": "text",
          "text": "Analyze user profile for trends"
        }
      ]
    },
    "metadata": {
      "accessCard": {
        "cardId": "ac-123",
        "agentId": "agent-abc",
        "dataSources": ["https://api.example.com/data"],
        "services": ["profile-analysis"],
        "expiration": "2025-12-31T23:59:59Z"
      },
      "transactionCard": {
        "cardId": "tc-456",
        "accessCardId": "ac-123",
        "permissions": ["read", "analyze"],
        "constraints": {
          "dataLimit": "100MB",
          "timeLimit": "2025-12-31T23:59:59Z"
        },
        "consent": "record-xyz"
      }
    }
  }
}
```

---

## ⚙️ How to Use

### 1. Validate Payloads

Use tools like [`ajv-cli`](https://ajv.js.org/) to validate A2A payloads locally:

```bash
ajv validate -s schemas/opnli_card_schema.json -d examples/sample_task_with_cards.json
```

### 2. Parse Metadata for Access Controls

If you're building an agent platform or A2A gateway:
- Look for `accessCard` and `transactionCard` inside the `metadata` object
- Enforce rules before sending a task to the target agent
- Log or verify the `consent` field if using audit or trust anchors

---

## 🔌 Extension Points in the A2A Protocol

This schema extends A2A in a non-breaking way by leveraging:

- `metadata` fields in:
  - `Task`
  - `TaskSendParams`
  - `Message`
- Optional structure under `params.metadata`

✅ This means agents and gateways that support CARDs remain **compatible** with baseline A2A.

---

## 🛡 Trust Model

CARDs serve as:
- ✅ **Proof of Authorization** (AccessCard)
- ✅ **Proof of Intent + Boundaries** (TransactionCard)
- 🛑 **Gatekeepers** for filtering or rejecting tasks that exceed granted limits

---

## 🧩 Integration Ideas

- **Agent Platforms**: Gate access by parsing cards before execution
- **Wallets or Agents-as-Apps**: Show users active CARDs and allow revocation
- **Gateways or Brokers**: Match tasks to agents based on access grants

---

## 📬 Feedback & Contributions

For schema feedback, ideas, or integration suggestions, open an issue or contact the [Opn.li project team](https://opn.li).