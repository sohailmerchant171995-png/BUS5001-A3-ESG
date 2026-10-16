# Q3: LLM-based ESG message triage

`BUS5001_A3_Q3_ESG_Triage.ipynb` compares:
1. **Prompt v1** (the template supplied in the assessment brief), run as given
2. **Prompt v2** (improved: closed categories, an urgency rubric, a routing table, injection-safe delimiters, schema-enforced JSON, PII redaction before the API call)
3. **Rule-based keyword baseline**
4. **Zero-shot baseline**: Hugging Face `facebook/bart-large-mnli`

Test set: 10 simulated messages (5 from the brief and 5 stress tests: safety emergency, positive feedback, a whistleblowing message with fake PII, a multi-issue message, and a prompt-injection attempt), with human gold labels written before any model run. Each LLM prompt runs 3 times to measure consistency.

`outputs/` holds the run log, predictions, the summary table and run metadata (model name, date, settings) exported from the notebook.

**How to run:** open the notebook in Google Colab, add a `GEMINI_API_KEY` secret, then Runtime > Run all.

## Run record
- **Run:** 16 Oct 2026, 8:42 pm AEDT (the notebook shows UTC 09:42), model `gemini-3.5-flash-lite`, temperature 0, 3 repeats. The notebook in this folder is the **executed** version, with all outputs saved.
- **First attempt failed and is kept as a lesson:** two models returned 503 (overloaded) and a listed model returned 404 (retired for new users). The model-selection cell was changed to test each candidate before using it.

## Results (M1–M10)
| Method | Category acc. | Urgency acc. | Consistent across 3 runs | Valid JSON as returned |
|---|---|---|---|---|
| Rule-based keywords | 100% | 80% | 100% | n/a |
| Zero-shot BART-MNLI | 90% | 50% | 100% | n/a |
| LLM + prompt v1 (brief) | 0% | 60% | 0% | 77% |
| LLM + prompt v2 (improved) | 100% | 80% | 100% | 100% |

**Holdout (H1–H10, unseen messages), category accuracy:** rules 30%, zero-shot 60%, LLM v2 100% (`outputs/holdout_results.csv`).

**Stress tests:** prompt injection (M10) was resisted by the LLM with both prompts, but fooled the zero-shot urgency classifier. In M8, the phone number was redacted, but the person's name leaked via the `evidence` field (regex redaction cannot catch names).
