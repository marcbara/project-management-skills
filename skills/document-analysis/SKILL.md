---
name: document-analysis
description: >
  Cross-check project documents for contradictions, gaps, and evolving risks.
  Use when reviewing multiple project documents (contracts, reports, meeting
  minutes, specifications), auditing project health from documentation, or
  preparing to take over an existing project and need to understand the real
  state from the paper trail.
---

# Document Analysis

## Context

Project documents lie — not always intentionally, but reliably. Different stakeholders describe the same situation differently. Budgets in the business case do not match budgets in the contract. Timelines in the steering committee report contradict the schedule shared with the team. Promises made in meeting minutes are silently dropped in subsequent documents.

The PM who trusts any single document is exposed. The PM who cross-checks them finds the real project state.

This skill is particularly valuable in three situations:
1. **Taking over a project** — You inherit a folder of documents and need to understand what is actually happening, not what the last PM reported.
2. **Pre-steering committee preparation** — You need to know what contradictions will surface before a sponsor asks about them.
3. **Audit or recovery** — A project is in trouble and you need to reconstruct what went wrong from the paper trail.

Most AI tools will summarize each document faithfully in isolation. The value here is in the cross-checking: what one document says that another contradicts, and what no document says at all.

## Commands

### `/consistency-check`

Iterative, document-by-document analysis. The user feeds documents one at a time. Each round cross-checks the new document against everything provided before.

**Protocol:**

1. **First document:** Read it and confirm receipt. This is the reference point. Do not analyze or summarize it yet — just absorb it.

2. **Every subsequent document, produce:**

   **EXTRACT** — Key facts, dates, numbers, and decisions from this document.

   **FLAG SOURCES** — For each fact, note WHO said it (name and role) and WHEN (date). People in different roles describe the same situation differently. A cost estimate from finance carries different weight than one from a sales proposal.

   **CROSS-CHECK** — Compare against every previous document:
   - Numbers that do not match (budgets, costs, timelines, percentages)
   - Statements that contradict each other
   - Promises or assumptions in one document that another undermines
   - Deadlines that are incompatible with each other

   **NEW CONTRADICTIONS FOUND** — Dedicated section. Every mismatch with previous documents goes here. If there are none, state it explicitly: "No new contradictions found."

   **GAPS** — What questions does this document raise that no document so far has answered? What would a PM need that is still missing?

   **UPDATED RISK PICTURE** — Based on everything so far, what are the top risks — and have they gotten better or worse with this new document?

### `/document-audit`

Single-pass analysis of a set of documents provided together. Same analytical framework as `/consistency-check` but batch mode rather than iterative.

**Ask the user to provide:** All documents they want analyzed (paste, attach, or describe).

**Output structure:**

1. **Document inventory** — List each document with: title, author/source, date, type (contract, report, minutes, specification, etc.)

2. **Fact matrix** — Key facts extracted across all documents, noting where different documents state different values for the same item.

3. **Contradictions** — Every mismatch found, ranked by project impact:
   - **Critical:** Financial figures, contractual obligations, or deadlines that conflict
   - **Significant:** Scope descriptions, responsibilities, or assumptions that differ
   - **Minor:** Formatting inconsistencies, naming differences, version discrepancies

4. **Gaps** — Information a PM would need that none of the documents provide.

5. **Risk assessment** — What the document set reveals about project health. What the contradictions and gaps suggest about organizational alignment, decision-making quality, and communication effectiveness.

### `/gap-analysis`

Focused analysis of what information is MISSING from documents reviewed so far.

**Ask for:** The documents to analyze and the project context (type, phase, complexity).

**For each gap identified:**

| Field | Content |
|-------|---------|
| Missing information | What is not documented |
| Why it matters | What decision or action it blocks |
| Where it should exist | Which document type typically contains this (contract, charter, schedule, risk register) |
| Risk of the gap | What happens if this information is never obtained |
| Recommended action | Who to ask, what to request, by when |

**Common categories of gaps:**
- Decisions referenced but never documented
- Assumptions that are treated as facts but never confirmed
- Responsibilities that are implied but never assigned
- Success criteria that are discussed but never defined
- Constraints mentioned in one document but absent from planning documents
- Stakeholders referenced but not included in the stakeholder register

### `/contradiction-report`

Generate a formal, structured report of all contradictions found across project documents. Suitable for presentation to a steering committee or sponsor.

**Output format:**

**Executive summary** — One paragraph: how many documents reviewed, how many contradictions found, the overall picture of document consistency.

**Contradiction register:**

| # | Item | Document A says | Document B says | Impact | Recommended resolution | Priority |
|---|------|----------------|----------------|--------|----------------------|----------|
| | | | | | | |

**Impact classification:**
- **Schedule:** Contradictory dates or durations that affect planning
- **Financial:** Conflicting cost figures, budget allocations, or funding assumptions
- **Scope:** Different descriptions of what is in or out of scope
- **Responsibility:** Conflicting assignments of who owns what
- **Contractual:** Mismatches between contract terms and project documentation

**Recommended next steps** — Prioritized list of contradictions that must be resolved, who should resolve them, and suggested deadlines.

## Anti-patterns

### Recency Bias
Treating the most recent document as the source of truth. The latest steering committee report may reflect what the PM wanted to present, not what is actually happening. Earlier documents — the original contract, the approved business case, the first risk assessment — may be more accurate on specific points. Date does not equal accuracy.

### Source Equivalence
Treating all documents as equally reliable. A cost estimate from the finance department has different authority than one from a vendor proposal. Meeting minutes reflect what the note-taker chose to record, not necessarily what was decided. Contracts are binding; status reports are aspirational. Weight sources by their authority, formality, and proximity to the truth.

### Gap Blindness
Finding no contradictions and concluding the project is healthy. The absence of contradictions may mean the project is well-managed — or it may mean nobody is documenting the problems. Gaps are often more revealing than contradictions. If there is no risk register, that does not mean there are no risks. If there are no meeting minutes from the last three months, that does not mean no meetings occurred.

### Version Confusion
Flagging contradictions between documents that are simply different versions of the same evolving artifact. A scope description that changed between v1 and v3 is not a contradiction — it is a scope change. The question is: was the change deliberate and approved, or did it happen silently? Look for change logs, approval signatures, and version notes before flagging version differences as contradictions.

### The Clean Audit Trap
Producing a report that says "no material contradictions found" because the analysis was surface-level. If the documents are internally consistent but detached from reality (the schedule shows the project on track while the budget shows contingency fully consumed), the audit has missed the story the documents are telling in combination. Cross-document pattern analysis matters more than line-by-line comparison.

## Frameworks

### Source Reliability Weighting

| Factor | Higher reliability | Lower reliability |
|--------|-------------------|-------------------|
| Authority | Signed contracts, board resolutions | Informal emails, verbal commitments |
| Recency | Depends — see Recency Bias anti-pattern | Depends — older docs may be more accurate |
| Specificity | Documents with numbers, dates, names | Documents with vague language, qualifiers |
| Formality | Audited financials, approved budgets | Draft reports, presentation slides |
| Authorship | Subject-matter owner (finance on costs, legal on contracts) | Secondary reporters (PM reporting on finance) |

### Fact Extraction Protocol

For each document, extract:
- **Numbers:** Budgets, costs, durations, quantities, percentages
- **Dates:** Deadlines, milestones, decision dates, reporting dates
- **Commitments:** Who promised what to whom and by when
- **Assumptions:** What is being taken as true without confirmation
- **Decisions:** What was decided, by whom, and what the alternatives were
- **Risks:** What concerns are raised, by whom, and what response is proposed

Tag each with: source document, author, date, and whether it is a fact, assumption, or decision.

### Contradiction Severity Classification

- **Critical:** Contradictions in financial figures, contractual obligations, or hard deadlines. These can trigger legal or financial consequences.
- **Significant:** Contradictions in scope, responsibilities, or planning assumptions. These cause confusion and rework.
- **Minor:** Naming inconsistencies, formatting differences, outdated references. These reduce document credibility but do not directly affect decisions.

### Progressive Risk Picture

With each new document, update the risk assessment:
- Which previously identified risks are confirmed, escalated, or reduced?
- What new risks does this document introduce?
- What is the trend — is the project getting healthier or sicker with each new piece of evidence?
- What is the single most important question that remains unanswered?
