# Opnli CARD Schema

This repository contains JSON schema definitions and usage examples for **AccessCard** and **TransactionCard**, two foundational components of the [Opn.li](https://opn.li) trust framework.

These CARDs are designed to extend the [Google A2A Protocol](https://github.com/google/A2A) by enabling agent-based access and control over personal or organizational data sources and services.

---

## 🔖 Schema Definitions

Located in [`schemas/opnli_card_schema.json`](schemas/opnli_card_schema.json), the schema includes:

### 📘 AccessCard

Used to authorize an agent to access specific data sources and services.

```json
{
  "cardId": "ac-123",
  "agentId": "agent-xyz",
  "dataSources": [...],
  "services": [...],
  "expiration": "...",
  "metadata": {...}
}
```

### 📗 TransactionCard

Defines the permissions and constraints on the agent’s access and actions.

```json
{
  "cardId": "tc-456",
  "accessCardId": "ac-123",
  "permissions": [...],
  "constraints": {...},
  "consent": "...",
  "metadata": {...}
}
```

---

## 🧪 Example Usage

An example task payload is available in [`examples/sample_task_with_cards.json`](examples/sample_task_with_cards.json). It demonstrates how to embed both CARDs into an A2A-style task message.

---

## 📚 Purpose

This schema is part of a larger effort to enable secure, user-defined, and verifiable agent-based access to data and services, especially for:

- Machine-consumable access control
- Privacy-respecting automation
- Transparent consent frameworks

---

## 🛠 How to Use

1. Clone this repository:
   ```bash
   git clone https://github.com/mikeoz/opnli-card-schema.git
   ```

2. Use a JSON schema validator to validate your payloads:
   ```bash
   npm install -g ajv-cli
   ajv validate -s schemas/opnli_card_schema.json -d examples/sample_task_with_cards.json
   ```

---

## 📄 License

This project is open-sourced under the MIT License.