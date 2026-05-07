# Product Requirements Document
## SQL Query Assistant for Ministry Platform

**Author:** Tamara Blanke, Data Specialist — Grace STL  
**Date:** May 7, 2026  
**Status:** Draft  

---

## 1. Problem Statement

> *This section explains WHY the product needs to exist. A strong problem statement is specific, grounded in real pain, and avoids jumping to solutions.*

Tamara frequently needs to write SQL queries directly against the Ministry Platform database to produce reports for leadership and staff. While Ministry Platform includes an in-tool query builder for simple queries, complex queries — especially those requiring table JOINs — must be written by hand in raw SQL.

Current AI tools (e.g., ChatGPT, Claude) can help with general SQL syntax but have no knowledge of the Ministry Platform schema: its table names, column names, relationships, or naming conventions. This means AI-generated queries routinely fail and require significant manual correction before they can be used.

**The result:** Reports take longer to produce, errors slip through, and Tamara is blocked on queries she could otherwise complete independently with the right support.

---

## 2. Goals

> *Goals describe what a successful outcome looks like — for the user and the organization. Keep them measurable where possible.*

| # | Goal |
|---|------|
| G1 | Tamara can describe a report need in plain English and receive a working, pasteable SQL query |
| G2 | Generated queries correctly reference Ministry Platform table and column names |
| G3 | JOIN logic is handled automatically based on schema relationships |
| G4 | Queries can be pasted directly into Ministry Platform's SQL view without modification |
| G5 | Leadership and staff receive more accurate reports, faster |

---

## 3. Non-Goals

> *Non-goals are just as important as goals — they define the boundaries of the project so it doesn't grow uncontrollably (called "scope creep").*

- This tool will **not** replace the Ministry Platform in-tool query builder for simple queries
- This tool will **not** write stored procedures or modify the database
- This tool will **not** connect directly to the live database (read-only schema context only)
- This tool will **not** be shared with or used by other staff in v1

---

## 4. Users

> *Who is this for? Being specific here prevents building the wrong thing.*

**Primary User: Tamara Blanke**
- Role: Data Specialist at Grace STL
- Technical level: Comfortable with concepts, struggles with complex SQL syntax (especially JOINs)
- Context: Produces reports for leadership and staff from Ministry Platform data
- Environment: Works inside Ministry Platform's SQL view interface

**Beneficiaries (not direct users):**
- Leadership and staff who receive reports generated from Tamara's queries

---

## 5. User Stories

> *User stories capture what a user wants to DO and WHY, written from their perspective. Format: "As a [user], I want to [action] so that [benefit]."*

| # | Story |
|---|-------|
| US1 | As Tamara, I want to describe a report in plain English so that I don't have to remember exact table and column names |
| US2 | As Tamara, I want the assistant to know my Ministry Platform schema so that generated queries use the correct table/column names |
| US3 | As Tamara, I want JOIN relationships handled for me so that I don't have to look up which tables connect to which |
| US4 | As Tamara, I want the output to be copy-paste ready so that I can use it immediately without editing |
| US5 | As Tamara, I want to ask follow-up questions like "add a filter for active members only" so that I can refine queries conversationally |

---

## 6. Functional Requirements

> *Functional requirements describe specific things the product MUST do. "The system shall..." language keeps these precise.*

| # | Requirement |
|---|-------------|
| FR1 | The assistant shall have access to the Ministry Platform schema (table names, column names, data types, and foreign key relationships) as context |
| FR2 | The assistant shall accept plain-English descriptions of desired report output |
| FR3 | The assistant shall return a complete, syntactically valid SQL SELECT statement |
| FR4 | The assistant shall explicitly reference correct JOIN syntax and conditions based on schema relationships |
| FR5 | The assistant shall be able to accept follow-up refinements to a previously generated query |
| FR6 | The assistant shall note when a requested field or table cannot be found in the known schema |

---

## 7. Non-Functional Requirements

> *Non-functional requirements describe HOW WELL the product must work — quality, performance, security, etc.*

| # | Requirement |
|---|-------------|
| NFR1 | Queries must be compatible with Ministry Platform's SQL dialect |
| NFR2 | The schema context must be maintainable — Tamara should be able to update it when tables change |
| NFR3 | The tool must not transmit live data — schema structure only, no actual member records |
| NFR4 | Response time should be fast enough for interactive use (under 30 seconds) |

---

## 8. Schema Context Strategy

> *This section is specific to this tool — it explains the most important technical decision: how the AI gets to "know" the database.*

The core challenge is giving an AI assistant accurate knowledge of the Ministry Platform schema without connecting it to the live database. The recommended approach:

1. **Export or manually document** the relevant tables, columns, and relationships from Ministry Platform into a structured file (e.g., `schema.md` or `schema.sql`)
2. **Include this schema file** in the AI's context window at the start of each session (via a CLAUDE.md reference, a system prompt, or a pasted block)
3. **Update the schema file** whenever tables or columns are added/changed

This approach keeps the tool simple, safe (no live data), and schema-accurate.

---

## 9. Success Metrics

> *How will you know if this worked? Metrics should be observable, not vague.*

| Metric | Target |
|--------|--------|
| Query accuracy | Generated queries run without syntax errors in Ministry Platform |
| Time savings | Tamara spends less time troubleshooting JOIN errors |
| Adoption | Tamara uses the tool for at least 80% of complex queries within 30 days of launch |
| Report quality | Fewer corrections requested by leadership on data reports |

---

## 10. Open Questions

> *Decisions that still need to be made. Don't let open questions block progress — document them and keep moving.*

| # | Question | Owner | Due |
|---|----------|-------|-----|
| OQ1 | What SQL dialect does Ministry Platform use under the hood? | Tamara | Before build |
| OQ2 | Which tables are used most frequently and should be documented first? | Tamara | Before build |
| OQ3 | Where will this tool live — Claude Code, a custom Claude project, or something else? | Tamara | Before build |
| OQ4 | Who approves the schema documentation for accuracy? | Tamara | Before build |

---

## 11. Out of Scope for v1 / Future Considerations

> *Ideas worth keeping but not building yet.*

- Auto-import schema directly from Ministry Platform API
- Query history and saved favorites
- Sharing the tool with other staff members
- Explaining what a query does in plain English (reverse direction)
- Visualizing query results

---

*This document is a living artifact. Update it as decisions are made and requirements change.*
