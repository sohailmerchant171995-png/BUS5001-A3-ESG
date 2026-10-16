# Q1: GreenDesk, an ESG Sustainability Support Assistant (Dialogflow CX)

**Use case:** staff report sustainability and facilities incidents (water leaks, energy waste, waste contamination, hazardous spills, accessibility barriers). The agent collects structured details, escalates safety risks, offers a human handoff, and redirects sensitive conduct or wellbeing matters to confidential channels without recording them.

## Build summary
| Item | Detail |
|---|---|
| Platform | Google Dialogflow CX (Conversational Agents console), flow-based agent |
| Region | `australia-southeast1` (Sydney), for Australian data residency |
| Intents | 8: `report_issue` (20 annotated phrases), `check_status`, `talk_to_human`, `sensitive_report`, `confirm_yes`, `confirm_no`, Default Welcome, Default Negative (out-of-scope examples) |
| Entities | `@issue_type` (5 values), `@campus_location` (8 values + exclusions), `@risk_level` (high / ongoing / low); fuzzy matching on, auto-expansion off |
| Pages | Start, Collect Details (form), Confirm Report, Urgent Escalation, Report Logged, Check Status, Human Handoff (live-agent handoff signal), Confidential Referral |
| Branching | `$page.params.status = "FINAL" AND $session.params.risk_level = "high"` -> Urgent Escalation; otherwise -> Confirm Report |
| Fallback | Two-strike reprompts on every form question, then handoff to a human; generative fallback deliberately **disabled** |
| Version | Backup `v1.0 tested` created 16 Oct 2026 |

## Files
- `GreenDesk_agent_export_FINAL.zip`: the full agent export (JSON package). To restore it: Agent settings > Restore.
- `agent_config/`: the same export unzipped, so intents, entities and pages can be read in the browser.
- `TESTING.md`: test transcripts (6 scenarios), the five test-driven fixes, and the configuration audit.

*Phone numbers, emails and the reference number in agent responses are placeholders for the prototype. In production, a webhook would create real ticket references in the organisation's service desk.*
