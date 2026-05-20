# Week 4: model comparison

Tested 4 AI models on 5 fraud investigation text samples to evaluate their usefulness for the fraud/anomaly detection component of our financial fraud detection project.

## Models tested

- HF Sentiment: distilbert-sst-2
- HF Zero-Shot: bart-large-mnli
- HF NER: bert-large-NER
- Groq Llama 3 8B

## Workflow summary

The n8n workflow reads 5 fraud-related test records from Airtable, sends each record to multiple AI models, and writes the model results back into Airtable.

The workflow includes:

1. Airtable Search Records
2. Hugging Face Sentiment Analysis
3. Hugging Face Zero-Shot Classification
4. Hugging Face Named Entity Recognition
5. Groq LLM Classification
6. Airtable Update Record

## Finding

Recommended Groq Llama 3 8B for the fraud/anomaly detection component because it gives a clear risk classification and a short explanation that is easier for investigators to understand.

See `report.md` for the full analysis.

## Files included

- `workflow.json` — exported n8n workflow
- `results/comparison-table.csv` — Airtable results export
- `report.md` — full model comparison report
- `README.md` — project summary
