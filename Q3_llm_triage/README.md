# Q3: LLM-based ESG message triage

`BUS5001_A3_Q3_ESG_Triage.ipynb` compares:
1. **Prompt v1** (the template supplied in the assessment brief), run as given
2. **Prompt v2** (improved: closed categories, an urgency rubric, a routing table, injection-safe delimiters, schema-enforced JSON, PII redaction before the API call)
3. **Rule-based keyword baseline**
4. **Zero-shot baseline**: Hugging Face `facebook/bart-large-mnli`

Test set: 10 simulated messages (5 from the brief and 5 stress tests: safety emergency, positive feedback, a whistleblowing message with fake PII, a multi-issue message, and a prompt-injection attempt), with human gold labels written before any model run. Each LLM prompt runs 3 times to measure consistency.

`outputs/` holds the run log, predictions, the summary table and run metadata (model name, date, settings) exported from the notebook.

**How to run:** open the notebook in Google Colab, add a `GEMINI_API_KEY` secret, then Runtime > Run all.
