# Q4: NotebookLM evaluation (climate-reporting briefing scenario)

**Scenario:** a postgraduate student prepares a board-workshop briefing on Australia's mandatory climate reporting.
**Sources loaded (public documents):** S1 ASIC *Regulatory Guide 280* (Mar 2025), S2 WRI/WBCSD *GHG Protocol Corporate Standard* (rev. ed.), S3 EY *Key elements in ASIC's RG 280* (May 2025).
**Run:** free version of NotebookLM ("Gemini Notebook"), 16 Oct 2026, 9:50–10:50 pm AEDT.

| File | Contents |
|---|---|
| `TEST_PLAN.md` | The planned tests and prompts, written before testing |
| `test_log.csv` | Scored log: 13 rows (12 tests run, 1 skipped), claims checked vs correct, hallucination, accuracy/relevance scores, key issue, evidence |
| `raw_outputs/` | Full outputs and fact-check notes for every test (`raw_T1.md` … `raw_T14.md`, plus full text of the T5 comparison and the T8 briefing doc) |

## Method
Each factual claim was checked against the source PDFs by full-text search. Two trap prompts were used: a **false premise** (T6) and an **out-of-scope** question about the EU CSRD (T7). Source balance was tested by comparing outputs generated **with** a specific topic prompt (flashcards, audio) and **without** one (briefing doc, mind map).

## Headline results
- 65 checked claims, **65 correct**; 0 fabricated facts; both traps passed.
- Without a topic prompt, the briefing doc and the mind map drew almost only on S1 (**source imbalance**). With a topic prompt, the outputs used S1 and S2.
- Other limitations: uncited extrapolations shown next to citations ("attribution blur"), an oversimplified flashcard, no citations in the Audio Overview, no flashcard export, a metered free tier, and wording that varies between runs.
