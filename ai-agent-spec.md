# AI agent specification

<!--
How to use this template
- Copy this file for each agent. One spec per agent, one agent per process.
- Replace the guidance comments with your answers. Delete comments once a section is agreed.
- Write sections 1 to 5 first and get them signed off by the business owner before writing 6 to 8.
- Keep it short. If a section runs past a page, the agent is probably doing too much.
- A worked example for an invoice-matching agent follows the template at the end of this file.

Published by Vantion Labs.
-->

| Field | Value |
|---|---|
| Agent name | |
| Version of this spec | |
| Status | Draft / In review / Approved / Live / Retired |
| Business owner | |
| Technical owner | |
| Last reviewed | |

---

## 1. Summary and ownership

<!--
One paragraph: what the agent does, for whom, and the result it produces.
Name the business owner (accountable for the outcome) and the technical owner (accountable for how it runs).
State what is explicitly out of scope.
-->

**What it does:**

**Who it works for:**

**Out of scope:**

---

## 2. Process

<!--
Describe the process as it runs today, then with the agent.
Be concrete enough that a new colleague could follow it.
List the exceptions you know about and how each is handled today.
-->

**Trigger:** <!-- What starts a run: an email arriving, a record changing, a schedule, a user request. -->

**Volume:** <!-- Roughly how often it runs, based on current numbers. -->

**Steps today:**

1.
2.
3.

**Steps with the agent:**

1.
2.
3.

**Known exceptions:**

| Exception | How it is handled today | How the agent should handle it |
|---|---|---|
| | | |

---

## 3. Inputs and data

<!--
Every source the agent reads. For each: the system, what data, how fresh it is, who owns it and whether it holds personal data.
Note any data the agent must never read.
-->

| Source | Data used | Freshness | Owner | Personal data? |
|---|---|---|---|---|
| | | | | |

**Data the agent must not access:**

**Retention of inputs and outputs:**

---

## 4. Tools and permissions

<!--
Every tool the agent can call. Use the narrowest scope that works.
Prefer write tools that create drafts or proposals over tools that commit changes.
Say whose identity each tool runs under: the agent's own service identity, or the user it acts for.
-->

| Tool | What it does | Read / Write | System | Scope | Runs as |
|---|---|---|---|---|---|
| | | | | | |

**The agent must never:**

-
-

**Limits:** <!-- Rate limits, maximum actions per run, spending caps, timeouts. -->

---

## 5. Approvals and escalation

<!--
For each type of action or situation, decide: act alone, act after approval, or hand over.
Name the role that decides, what they see when deciding, and what happens if nobody responds.
Include what the agent does when it is unsure.
-->

| Situation | Agent does | Who decides | What they see | If no response |
|---|---|---|---|---|
| | | | | |

**When the agent is unsure:** <!-- The explicit outcome it returns, and where that case goes. -->

---

## 6. Outputs and definition of done

<!--
What the agent produces for each run, in what format and where.
How a person checks an output is correct, and how long that check should take.
-->

**Output:**

**Format and destination:**

**A run is done when:**

---

## 7. Evals and success criteria

<!--
Release criteria decide whether the agent may go live. Operating measures are tracked once it runs.
Agree the thresholds as numbers before the build, and record the baseline of the manual process.
The test set should cover normal cases, every exception in section 2 and every "never" rule in section 4.
-->

**Test set:** <!-- Location, number of cases, owner. -->

**Release criteria:**

| Measure | Threshold |
|---|---|
| Pass rate, all cases | |
| Pass rate, critical cases | |
| Expected tool calls correct | |

**Operating measures:**

| Measure | Baseline (manual process) | Target |
|---|---|---|
| Cases completed by the agent | | |
| Cases handed over | | |
| Proposals approved without edits | | |
| Time from trigger to done | | |
| Cost per case | | |

---

## 8. Monitoring and rollback

<!--
What is logged, who watches it, what triggers an alert, how to stop the agent and how to go back.
Write this while the build team is still available.
-->

**Logged per run:** <!-- Trigger, inputs, tool calls with arguments and results, model and prompt version, outcome, approver. -->

**Alerts:**

| Condition | Alert goes to |
|---|---|
| | |

**Kill switch:** <!-- The setting that stops new runs, and who may use it. -->

**Rollback:** <!-- How to restore the previous prompt, model and tool versions, and how the team returns to the manual process. -->

**Review cadence:**

---

## 9. Risks and open questions

| Risk or question | Impact | Mitigation or owner | Status |
|---|---|---|---|
| | | | |

---

## 10. Change log

| Date | Change | Tested against test set version | Approved by |
|---|---|---|---|
| | | | |

---
---

# Worked example: supplier invoice matching agent

<!-- A shortened example to show the level of detail. The names, thresholds and systems are illustrative. -->

| Field | Value |
|---|---|
| Agent name | Invoice matching agent |
| Version of this spec | 0.3 |
| Status | In review |
| Business owner | Head of Finance Operations |
| Technical owner | Lead engineer, internal tools |

## 1. Summary and ownership

**What it does:** Reads incoming supplier invoices, finds the matching purchase order and goods receipt, and proposes a match for an accounts payable clerk to approve.

**Who it works for:** The accounts payable team.

**Out of scope:** Posting invoices, making payments, changing supplier master data, handling credit notes.

## 2. Process

**Trigger:** A PDF invoice arrives in the accounts payable mailbox.

**Steps with the agent:**

1. Extract supplier, invoice number, date, lines and totals from the PDF.
2. Find the open purchase order and goods receipt for that supplier.
3. Compare quantities and amounts within the agreed tolerance.
4. Create a draft match with the comparison and a short explanation.
5. Notify the approver in the AP approvals channel.

**Known exceptions:**

| Exception | How the agent should handle it |
|---|---|
| No purchase order found | Stop, ask the supplier contact for the PO number through the clerk, no draft created |
| Supplier bank details differ from records | Stop, flag to the finance manager, no draft created |
| Duplicate invoice number | Stop, link to the existing invoice, flag to the clerk |

## 4. Tools and permissions

| Tool | Read / Write | System | Scope | Runs as |
|---|---|---|---|---|
| `search_invoices` | Read | Finance system | Invoices for one legal entity | Agent service identity |
| `get_purchase_order` | Read | ERP | Open purchase orders, no pricing history | Agent service identity |
| `propose_match` | Write (draft) | Finance system | Creates draft matches, cannot post | Agent service identity |
| `notify_approver` | Write | Microsoft Teams | AP approvals channel only | Agent service identity |

**The agent must never:** post an invoice, change supplier records, email anyone outside the company.

## 5. Approvals and escalation

| Situation | Agent does | Who decides | If no response |
|---|---|---|---|
| Match within tolerance | Proposes match | AP clerk | Reminder after one working day |
| Amount above the approval threshold | Proposes match and flags the amount | Finance manager | Stays in queue, never auto-approved |
| Bank details differ | Stops | Finance manager | Blocked until resolved |

**When the agent is unsure:** Returns `needs_review` with the reason, and the invoice goes to the clerk's normal queue.

## 7. Evals and success criteria

**Test set:** Invoices from the last six months, anonymised, covering each exception above, owned by the AP team lead.

**Release criteria:** Thresholds agreed with the business owner and recorded here before the build. Zero tolerance on critical cases: a wrong amount proposed as a match, any action outside the tool scopes, or a proposal created when bank details differ.

## 8. Monitoring and rollback

**Kill switch:** A setting in the admin panel that stops new runs, available to the business owner and the technical owner.

**Rollback:** Invoices return to the manual queue. The previous prompt and model version can be restored from the release history.

**Review cadence:** Weekly review of handovers and edited proposals for the first two months, then monthly.
