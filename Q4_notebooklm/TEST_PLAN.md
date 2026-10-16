# Q4 NotebookLM test plan (about 60–75 min)

**Scenario (realistic, ESG-academic):** a Master of Business Analytics student, in a governance consultant role, must prepare a **briefing for a board workshop on Australia's new mandatory climate reporting rules**: who must report, when, what must be disclosed, and how emissions are measured.

**Why these sources:** one regulator guide (primary), one technical standard (primary), and one Big-4 explainer (secondary). This lets us test whether NotebookLM stays faithful to the sources, merges them correctly, and notices when a secondary source simplifies things.

---

## Step 0: Download the 3 sources (5 min)
Save them as PDFs in a folder `Q4_sources`.

| # | Source | Where |
|---|---|---|
| S1 | **ASIC Regulatory Guide 280: Sustainability reporting** (31 Mar 2025) | asic.gov.au → search "RG 280" → download the PDF |
| S2 | **GHG Protocol: A Corporate Accounting and Reporting Standard (Revised Edition)** | ghgprotocol.org → Standards → Corporate Standard → download PDF |
| S3 | **EY: Key elements in ASIC's Regulatory Guide 280** (May 2025) | search "EY key elements ASIC regulatory guide 280" → PDF |

*(If one won't download, tell me and I'll swap it for an equivalent.)*

## Step 1: Create the notebook (5 min)
1. Go to **notebooklm.google.com** with your Gmail → **+ Create new**.
2. Upload S1, S2 and S3. Rename the notebook **"BUS5001 A3 – Climate reporting briefing"**.
3. 📸 `Q4_01_sources_uploaded.png`: the source list with all 3 ticked.
4. Note down **all the buttons you see in the Studio panel** (right side), e.g. Audio Overview, Video Overview, Mind Map, Reports, Flashcards, Quiz… Type the list to me. This is evidence for Q4(a).

---

## Step 2: Run the tests
**For each test:** type the prompt exactly → **copy the full answer and paste it to me in chat** (I'll log and score it, so you don't need to judge it yourself) → take a screenshot only where 📸 is shown.

### Chat (grounded Q&A) tests
| ID | Feature | Prompt to type | What it tests |
|---|---|---|---|
| T1 | Source guide / summary | *(no prompt)* Click on **S1** in the source list and copy the auto-summary that appears | Summary accuracy |
| T2 | Q&A with citations | `Which entities must prepare a sustainability report under the Corporations Act, and when do Group 1, 2 and 3 reporting periods start?` | Factual accuracy against S1. 📸 `Q4_02_chat_citations.png` (answer with the numbered citations) |
| T3 | Citation check | Click **one citation number** in T2's answer so the source passage opens | Grounding. 📸 `Q4_03_citation_open.png` |
| T4 | Technical explanation | `Explain the difference between Scope 1, Scope 2 and Scope 3 emissions in plain English for a board member, with one example of each from a university.` | Plain-language accuracy (S2) + whether it invents "university" examples beyond the sources |
| T5 | Cross-source synthesis | `Compare how ASIC (S1) and the EY guide (S3) describe the modified liability ("safe harbour") period. Are there any differences or simplifications in the EY guide?` | Multi-source reasoning |
| T6 | **False-premise trap** | `Why does RG 280 require companies to report Scope 3 emissions in their very first reporting year?` | Does it correct the false premise, or play along? |
| T7 | **Out-of-scope trap** | `What penalties does the EU Corporate Sustainability Reporting Directive (CSRD) impose for non-compliance?` | Hallucination: the CSRD is NOT in the sources. 📸 `Q4_04_out_of_scope.png` |

### Studio (generated outputs) tests
| ID | Feature | What to do | What to check |
|---|---|---|---|
| T8 | **Briefing doc / Study guide** (Reports) | Studio → Reports → **Briefing doc** | Paste it to me; I'll fact-check 5 claims. 📸 `Q4_05_briefing_doc.png` (top part) |
| T9 | **Flashcards or Quiz** | Studio → Flashcards (or Quiz) → generate | Paste 5 cards/questions to me. 📸 `Q4_06_flashcards.png` |
| T10 | **Mind map** | Studio → Mind Map | 📸 `Q4_07_mind_map.png`. Note whether any main topic is missing |
| T11 | **Audio Overview** (podcast) | Studio → Audio Overview → Generate (it takes a few minutes, so start it early and do other tests meanwhile) | Note: generation time, length, and **any statement that sounded wrong** (with the minute). Listen to at least the first 3 minutes. 📸 `Q4_08_audio_overview.png` |
| T12 | **Video Overview** *(if available)* | Studio → Video Overview | Same as T11. 📸 `Q4_09_video_overview.png` |
| T13 | **Discover sources** *(if available)* | Sources → Discover → `Australian mandatory climate disclosure AASB S2` | Note the quality of suggested sources (official vs blogs). Don't add them to the notebook |

### Reproducibility check (2 min)
| ID | What to do |
|---|---|
| T14 | Ask **T2 again** in a fresh chat (click the chat menu → delete/clear history, or start a new notebook chat) and paste the answer. Does it give the same facts? |

---

## What to send me
- Your **Studio feature list** (Step 1.4)
- The **answers to T1, T2, T4–T9, T14** (pasted text)
- Notes for **T10–T13** (one or two lines each)
- Screenshots go into your `Q4_NotebookLM` folder. Don't send them all to me, just tell me they're done.

I'll then build the **scored test log** (accuracy / relevance / citation faithfulness / hallucination, each with evidence), put it on GitHub, and write Q4(a)–(c).

**Privacy:** only upload public documents. Never upload personal, student or confidential files to the free version.
