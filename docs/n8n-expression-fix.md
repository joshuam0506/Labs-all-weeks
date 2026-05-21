# n8n Expression Fix — Airtable Update Record

## Problem

During the AI Core workflow, n8n was able to call Hugging Face and Groq successfully, but Airtable was not always updating the correct fields. Some rows stayed blank, and there was a risk of accidentally overwriting original transaction data such as `amount`, `transaction_id`, or `status`.

The main issue was in the Airtable **Update Record** node. The workflow needed to update only the AI output fields while matching the correct Airtable record ID.

## Workflow Context

The workflow reads records from the Airtable `Transactions` table, sends transaction details to Hugging Face and Groq, then writes the AI results back into Airtable.

Important fields updated by n8n:

- `HF Label`
- `HF Confidence`
- `Groq Analysis`
- `AI Processed`

Original transaction fields should not be overwritten.

## Correct Record Match Expression

In the Airtable **Update Record** node, the record should be matched using the Airtable record ID from the `Search records` node.

```javascript
{{ $('Search records').item.json.id }}
