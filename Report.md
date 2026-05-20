# Model comparison report — week 4

**Name:** Joshua M  
**Date:** 2026-05-20  
**Capstone project:** financial fraud detection  
**My component:** fraud/anomaly detection and transaction risk analysis  

## Test setup

**Input dataset:** 5 fraud investigation text samples covering:

- 2 clearly concerning or high-risk records
- 1 ambiguous record that may require review
- 2 routine or lower-risk records

**Models tested:**

1. distilbert-base-uncased-finetuned-sst-2-english — sentiment analysis
2. facebook/bart-large-mnli — zero-shot classification
3. dslim/bert-large-NER — named entity recognition
4. Groq llama-3.1-8b-instant — LLM risk classification

**Evaluation criteria:** label accuracy, confidence score, usefulness of output, speed, and ease of integration in n8n.

## Results summary

| record | sentiment | zero-shot | ner entities | Groq |
|--------|-----------|-----------|--------------|------|
| 1. Wire transfer of $47,500 to Lagos from a new device | NEGATIVE | requires review | Chase Bank, Lagos | HIGH — the transaction shows strong fraud indicators because it involves a large transfer, a new device, a new account, and an international destination. |
| 2. Monthly salary deposit of $4,200 from known employer | POSITIVE / routine leaning | routine transaction | no major useful entities | ROUTINE — this record appears normal because it matches an expected salary deposit from a known employer. |
| 3. Credit card used for 12 transactions in 4 countries within 6 hours | NEGATIVE | requires review | David Kim, Miami | HIGH — this record suggests possible card compromise because the card was used in multiple countries while the cardholder was in Miami. |
| 4. Loan application to Wells Fargo with same Chicago address as 8 other applications | NEGATIVE | requires review | Wells Fargo, Chicago | HIGH — this record may indicate application fraud because multiple applicants used the same address. |
| 5. ATM withdrawal of $200 at customer’s home branch | POSITIVE / routine leaning | requires review or routine | no major useful entities | LOW or ROUTINE — this record appears mostly normal because it is a small withdrawal at the customer’s home branch. |

## Analysis

**Where models agreed:**  
The models mostly agreed that the wire transfer, credit card activity, and loan application records were suspicious or required review. Groq gave the clearest fraud explanation because it connected the risk factors to real investigation logic, such as large transfers, new accounts, international destinations, and unusual transaction patterns.

**Where models disagreed:**  
The biggest disagreement was with the ATM withdrawal. Some models still marked it as requiring review, even though the transaction looked normal because it was only $200 and happened at the customer’s home branch. This shows that simple classification models may over-flag normal activity when they do not fully understand the context.

**Most accurate model overall:**  
Groq was the most accurate overall because it gave a risk level and a short explanation. It was better than the basic sentiment model because fraud detection is not just about whether text sounds negative or positive. It also requires understanding context.

**Fastest/most practical:**  
The Hugging Face zero-shot model was practical because it gave clear labels and confidence scores. However, Groq was more useful for investigation because it explained why the transaction was risky or routine.

## Recommended models for my capstone component

**Component:** fraud/anomaly detection and transaction risk analysis

**Primary model:** Groq llama-3.1-8b-instant — I recommend this model because it gives a clear fraud risk classification and explains the reason in plain language.

**Secondary model:** Hugging Face zero-shot classification — I would use this model as a supporting model because it gives a quick label and confidence score that can help decide whether a transaction should be reviewed.

**Rejected models and why:**

- sentiment analysis: this model is not the best fit because fraud detection is not the same thing as positive or negative emotion. A normal sentence can sound negative, and a suspicious transaction can be written in neutral language.
- named entity recognition: this model is useful for extracting names, places, and organizations, but it does not classify fraud risk by itself. It should be used as a support tool, not the main decision model.

## Failure cases and limitations

One limitation was that some normal transactions could still be flagged as requiring review. For example, the ATM withdrawal at the customer’s home branch was a normal-looking transaction, but a model may still flag it because it sees financial activity and treats it as suspicious. This shows that a fraud detection system should not automatically block or accuse someone based only on an AI label. A human investigator should review uncertain cases.

Another limitation is that NER only extracts entities like names, locations, and organizations. If a record does not include those details, the NER model may return little or no useful information. This is expected, but it means NER is only helpful for certain types of records.

## Next steps

If I had more time, I would test more than 5 fraud records and include more examples of normal, suspicious, and clearly fraudulent transactions. I would also test different zero-shot labels, such as “low risk,” “medium risk,” “high risk,” and “critical risk.” Finally, I would compare Groq with another LLM to see which model gives the most reliable fraud explanations.

## Reflection

What surprised me most was that each model was useful in a different way. The sentiment model was simple but not very specific for fraud. The zero-shot model was better because it could use fraud-related labels. NER was helpful for pulling names, banks, and locations, but it could not decide risk by itself. Groq was the most useful because it gave both a classification and an explanation, which makes it easier for a fraud investigator to understand the result.
