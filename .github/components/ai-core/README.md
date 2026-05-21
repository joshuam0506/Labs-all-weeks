# AI Core Component — Fraud Detection Analysis

## What this component does

The AI Core component analyzes financial transaction records to help detect possible fraud. It reads transaction data from Airtable, sends the information to AI models through n8n, and writes the results back into Airtable.

The goal is not to automatically accuse a customer of fraud. The goal is to flag suspicious records for review and give investigators a clearer explanation of why a transaction may need attention.

## Inputs

This component reads records from the `Transactions` table in Airtable.

Main input fields include:

- `transaction_id`
- `timestamp`
- `sender_id`
- `recipient_id`
- `amount`
- `transaction_type`
- `description`
- `status`
- `error_reason`

## Outputs

The AI Core writes results back into Airtable using fields such as:

- `HF Label`
- `HF Confidence`
- `Groq Analysis`
- `AI Processed`

These fields help show whether a record was analyzed, how confident the model was, and why the transaction may be suspicious.

## How it connects to other components

### Ingestion → AI Core

The ingestion component creates or imports transaction records into Airtable. Once records are available, the AI Core workflow in n8n reads those records and sends them to AI models.

### AI Core → Specialist

After AI Core analyzes the transaction, suspicious records can be sent for specialist review. A high-risk or unusual transaction may become an investigation case.

### AI Core → Integration

The integration component uses Airtable as the shared system. Updated AI fields allow the rest of the project to see which records have been processed and which ones need review.

## Tools used

- Airtable for storing transactions
- n8n for workflow automation
- Hugging Face for AI classification or confidence scoring
- Groq for fraud risk explanation
- GitHub for documentation

## Basic workflow

1. n8n searches Airtable for transaction records.
2. n8n sends transaction details to Hugging Face.
3. Hugging Face returns a label and confidence score.
4. n8n sends transaction details to Groq.
5. Groq returns a plain-language fraud risk analysis.
6. n8n updates Airtable with the AI results.
7. Records that look suspicious can be reviewed by the specialist component.

## How to test it

1. Add a test transaction to Airtable.
2. Make sure the transaction has the required fields filled in.
3. Run the n8n workflow manually.
4. Check that the workflow reads the Airtable record.
5. Check that Hugging Face and Groq return results.
6. Check that Airtable updates the fields:
   - `HF Label`
   - `HF Confidence`
   - `Groq Analysis`
   - `AI Processed`

## Example test record

```text
Transaction ID: TXN-TEST-001
Amount: 7800
Transaction Type: transfer
Description: transfer transaction of $7800 from USR_006513 to USR_081245
Status: pending
