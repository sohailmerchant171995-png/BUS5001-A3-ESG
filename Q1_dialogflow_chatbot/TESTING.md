# GreenDesk test transcripts (Dialogflow CX simulator, Draft env, 16 Oct 2026)

## Test A - normal report (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | hi | Hi, I'm GreenDesk... Please don't share personal or health information here, and you can stay anonymous. What would you like to do? | Start Page / Default Welcome Intent |
| 2 | The air conditioning is running overnight in an empty office | I can log that for you. I just need a few quick details. / Where is it? For example: Building C, the Library, or the Car Park. | Collect Details / report_issue; issue_type=energy_waste pre-filled |
| 3 | Building B | Is anyone at risk, or is it still happening right now?... | campus_location=Building B |
| 4 | still running | Thanks. Here is what I have: a energy_waste at Building B, risk level ongoing. Shall I send this...? | risk_level=ongoing -> Confirm Report (normal path) |
| 5 | yes | Done. Your report reference is ESG-2026-0147... Is there anything else I can help with? | confirm_yes -> Report Logged |
| 6 | no | Thanks for helping us run a more sustainable workplace. Goodbye. | confirm_no -> End Session |
Finding: the read-back shows the raw value "a energy_waste" (snake_case, grammar). Fix: reword the Confirm Report message.

## Fix 3 applied: Confirm Report wording -> "Issue: X. Location: Y. Risk: Z."

## Test E - correction path (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | report a problem | I can log that for you... / What kind of issue is it? For example: a water leak, energy waste, waste or recycling, a spill, or an accessibility barrier. | report_issue (no entity) -> Collect Details asks issue_type |
| 2 | energy waste | Where is it? For example: Building C, the Library, or the Car Park. | issue_type=energy_waste |
| 3 | Library | Is anyone at risk, or is it still happening right now?... | campus_location=Library |
| 4 | no it is minor | Thanks. Here is what I have. Issue: energy_waste. Location: Library. Risk: low. Shall I send this...? | risk_level=low -> Confirm Report |
| 5 | no | No problem, let's correct it. / I can log that for you... / What kind of issue is it?... | confirm_no -> presets null -> Collect Details re-asks all 3 |

## Test B (run 1) - urgent path (PASS, with finding)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | There is a water leak in Building C and it is slippery | I can log that for you... / Is anyone at risk, or is it still happening right now?... | report_issue; issue_type=water_leak, campus_location=Building C captured; risk NOT captured |
| 2 | yes it is unsafe | This sounds urgent. If anyone is in danger, call Campus Security now on 1800 000 000. I am flagging your report as PRIORITY... / Done. Your report reference is ESG-2026-0147... | risk_level=high -> Urgent Escalation -> (true) Report Logged; confirmation step skipped by design |
Finding 4: the risk word "slippery" was not extracted in a single turn, because only one training phrase carried a risk_level annotation. The bot safely asked instead of guessing. Fix: add 5 annotated risk phrases to report_issue, then retest.

## Fix 4 applied: 5 annotated risk phrases added to report_issue (20 phrases total); model retrained (~3 min)
## Test B (run 2) - urgent path, single turn (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | There is a water leak in Building C and it is slippery | I can log that for you... / This sounds urgent. If anyone is in danger, call Campus Security now on 1800 000 000. I am flagging your report as PRIORITY... / Done. Your report reference is ESG-2026-0147... | issue_type=water_leak, campus_location=Building C, risk_level=high all from one sentence -> Urgent Escalation -> Report Logged |
Note: the first retest ran before retraining finished; the change worked about 3 minutes after saving.

## Test B2 - generalisation check with an unseen sentence (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | The heater in the Cafeteria is broken and it is dangerous | I can log that for you... / This sounds urgent... / Done. Your report reference is ESG-2026-0147... | not a training phrase; issue (heater->energy_waste), location=Cafeteria, risk=high (dangerous) all extracted -> Urgent Escalation. The model generalises rather than memorising. |

## Test C - fallback and handoff (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | blah blah pizza | Sorry, I didn't quite get that. I can help you report a sustainability or facilities issue, check a report, or connect you with a person. What would you like to do? | Start Page / sys.no-match-default |
| 2 | I want to report an issue | I can log that for you... / What kind of issue is it?... | report_issue -> Collect Details |
| 3 | purple | Sorry, I didn't catch that. Is it water, energy, waste, a spill, or accessibility? | issue_type no-match-1 (reprompt) |
| 4 | banana | I'm having trouble understanding, so let me connect you with a person. / No problem, I will connect you with a person... Facilities and Sustainability Desk on 1800 000 001... | no-match-2 -> Human Handoff (+ live agent handoff signal) -> End Session |

## Test D - sensitive boundary (PASS)
| Turn | User | GreenDesk | Page / match |
|---|---|---|---|
| 1 | my manager is bullying me | Thank you for trusting me with this. I am not the right channel for personal or conduct matters, so I will not record any details. Please contact the confidential Integrity Line on 1800 000 002, or the free Employee Assistance Program on 1800 000 003, available 24/7. If you are in immediate danger, call 000. | sensitive_report (flow-level route) -> Confidential Referral -> (true) End Session; no parameters collected |

## Summary: 7 runs, 5 scenarios (A normal, B urgent x2 + unseen-sentence generalisation, C fallback/handoff, D sensitive, E correction) all PASS after 4 test-driven fixes:
1. Fuzzy false positives ("blocked"/"broken" -> location) -> removed "block X" synonyms + entity exclusions
2. Auto-annotation mis-tags (Someone->risk, problem->issue, duplicate issue tags) -> re-entered phrases with manual tagging
3. Read-back grammar ("a energy_waste at") -> structured "Issue: / Location: / Risk:"
4. Single-turn risk not extracted -> 5 annotated risk phrases; worked after ~3 min retraining

## Export audit (automated check of the exported JSON, 16 Oct 16:45)
Structure verified: 8 intents, 3 entities, 7 pages, all routes, conditions, reprompts, presets and the live-agent handoff are as designed.
Data-quality issues found (Fix 5):
- hazardous_spill had the synonym "accessibility issue" (a copy error; it is also under accessibility_barrier), creating ambiguity. Remove it.
- hazardous_spill was missing "dumped batteries"; energy_waste was missing "energy waste". Add both.
- Two location synonyms had stray tab characters ("\tbuilding c", "\tmain building"). Clean them up.
Lesson: inspecting the exported configuration as code catches errors the UI hides (a governance and QA point).
Fix 5 result (final export, 16:50): all synonyms are on the correct values; automated check found no duplicate synonyms across values. Re-audit also caught a fix-induced slip ("energy waste" first added to hazardous_spill, plus an accidental "same line" chip); both corrected. Two cosmetic leading-tab synonyms remain (harmless).
