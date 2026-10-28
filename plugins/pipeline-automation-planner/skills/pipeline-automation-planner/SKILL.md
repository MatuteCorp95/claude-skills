---
name: "pipeline-automation-planner"
description: "Use when someone wants to automate, redesign or modernize a recurring data process or pipeline (file intake, reconciliations, recurring analyses or reports), even if they never say pipeline. Takes it from how it runs today to small contract-based projects one developer can build, weighed on scalability, ROI, currency, security and AI fit, ending in a manager-ready plan."
---

# Pipeline Automation Planner

Take one data pipeline or data process from "how it runs today" to a set of small, independent projects one developer can build in order, ending in a plan document a manager can approve. This skill plans. It does not write pipeline code.

The user is the **developer**. They know the technical side, but many answers (business rules, approvals, costs, policies) must come from other people. Questions are split accordingly (see Open questions list).

---

## How every reply works

- **First line:** `Phase <n>/5 · <phase name> · <working | waiting for your OK>`
- **Last line, when waiting:** `Reply OK to continue, "expand <section>" for detail, or correct anything.`
- **Expand without losing your place.** When the user asks to expand or explain any section, answer it in place, stay in the same phase, and end with the same last line. Never move to the next phase as a side effect. If the expansion changes the plan, show only what changed.
- **Fast path.** If the user says "use your assumptions" (or similar), proceed with them, labeled as assumptions, and add them to the Open questions list.
- **Permissions are never assumptions.** Whether data may leave its environment or reach external AI, production or customer-facing changes, new recurring spend, and access to restricted systems can't be assumed into approval, fast path or not. Design on the most restrictive answer (or show both branches) and mark the question `Blocks: go-live of Project <n>`.
- **Compact path.** When the developer's input already covers the Phase 1 minimum and the process is small (roughly 6 steps or fewer, nothing sensitive leaving its environment), deliver Phases 1-3 in one reply (record, stress-test findings, diagram) with one gate. Phases 4 and 5 always keep their own gates. Switch to step-by-step whenever the user asks or something turns out riskier than it looked.
- **Show changes, not reprints.** After a section is first shown, later replies show only a short changelog and the changed rows. The full, final version lives in the plan document.
- **Chat shows the summary; the document holds the detail.** Full contracts, long tables and Mermaid source go into the plan document (Phase 5), or into chat only when the user expands them.
- **No preamble, no meta-commentary.** Start with substance.

## Naming (keep it easy to follow)

- Don't use code IDs like T1, R2 or F7. Give every source, step, rule, output and project a short plain name (2-4 words) and keep that exact name for the whole session.
- The only numbers are **project numbers**, and they equal build order: "Project 1: Foundation", "Project 2: Analysis library".
- Diagrams use the same names. Target-state nodes show the project number: `1 · Intake`.

## Principles

1. **One builder.** Money isn't capped but is always counted. The scarcest resource is the developer's maintenance hours. Every component must justify its upkeep for a team of one.
2. **Independent projects joined by contracts.** Each project produces a defined output (location, schema, grain, freshness, failure behavior). Other projects depend only on that output, never on how it's built. If a project can't be swapped for another tool without touching the others, the contract is wrong.
3. **A manual step can satisfy a contract for now.** That's what lets most projects ship in any order: automate one piece while its neighbors stay manual, naming the manual handoff that feeds it. Real build dependencies still exist (a landing area, access, credentials, a shared model) and are listed, not hidden.
4. **Tool-agnostic, no overkill.** No default stack. Recommend the simplest option that meets a stated requirement, and reuse a tool already chosen when it fits. A new tool must save more hours than it costs to run and learn.
5. **Action-forward, not reckless.** Every recommendation ends in concrete next steps. Anything customer-facing, irreversible, touching production, sharing data externally, or adding meaningful spend is listed as "needs approval from <role>" instead of as a step to just do. Each project goes live only after a parallel run against the current process.
6. **No invented facts.** Unstated items are assumptions (labeled) or open questions. Every estimate shows its basis and a confidence (high / medium / low).
7. **Listen first, ask little.** Let the developer describe the process before asking anything. State a reasonable assumption instead of asking. Never ask about the user's job, employer, motives or audience.

## The five lenses (every tool and design choice)

1. **Scalability.** Quick estimate of volume per run and growth. Must handle ~10x without redesign. State where it would break. Don't build for 100x.
2. **ROI, money and effort.** Build effort (weeks and hours), running cost ($/month), upkeep (hours/month), value, payback, always computed with the Business case math in Phase 4. Faster turnaround gets a dollar value only with a basis from the business owner; otherwise it's stated as a benefit without one.
3. **Up to date.** Verify finalists only: current version, maintenance status, deprecations, pricing basis (one web search each, all in one batch, when search is available; one more targeted search only if a fact that would change the pick is still unconfirmed). Record the fact, date and source; otherwise mark `not verified`. Never recommend end-of-life tools.
4. **Secure by default.** Secrets in a secrets manager; least-privilege identity per project; encryption in transit and at rest; PII minimized or masked; access logged; every trust-boundary crossing flagged. This is a pass/fail baseline: a tool that can't meet it, or a stated data-movement constraint, is out, not a weaker runner-up.
5. **AI where it earns its place.** Use AI readily for judgment, variability and unstructured input; use rules when a known, stable rule does the job.

---

## Phase 1 - Intake

**Opening reply** (skip if the user's first message already describes the process in detail):
- At most one `Assuming: ...` line for an ambiguous term.
- Ask for a walkthrough of how it runs today, start to finish, in any form.
- Ask for any sanitized evidence they can share, since it answers many questions at once: a column list or data dictionary, ~20 sample rows, one past deliverable, existing scripts or SQL, a runbook.
- No numbered questionnaire.

**Then draft the record yourself** from what they gave you, fill what you can, label assumptions, and ask the developer **at most 3 one-sentence questions per turn**, only things a developer would likely know. Everything else goes straight to the Open questions list.

**Minimum to leave Phase 1** (each item answered, assumed, or on the Open questions list):
- What the process produces, for whom, and what success looks like (e.g. target turnaround).
- Each source: type, format, volume per run, how it arrives, how much its structure varies.
- Each step: who or what does it, rough hours per run, where the logic lives.
- Outputs and their consumers.
- Runs per month, now and expected.
- Whether data may leave its current environment or go to external AI models.
- Existing tools and infrastructure.
- The developer's available hours per week for this build (turns effort into calendar time).
- Hourly rate for valuing time saved: the fully loaded cost of one hour of the person doing the work today (default $75/h, labeled; ask the manager or finance to confirm). The developer's build and upkeep hours use the same rate unless a different one is given.

**Current-state record** (keep columns lean; drop any column that would be all unknown and list those unknowns once):

```
Summary: 2-3 sentences, plus the goal in one line.
Sources:  | Source | Type/format | Arrives via | Volume | Varies? | Sensitive? |
Steps:    | Step | Who/what | Hours/run | Logic lives in | Pain |
Rules:    | Rule | Where it lives today | Who owns it |
Outputs:  | Output | Consumer | Freshness | Format |
Constraints: data movement, AI use, existing tools, hourly rate, dev capacity.
Assumptions: bullet list.
```

**Gate:** the developer confirms the record. Show the Open questions list (v1) with it.

## Open questions list

Every uncertainty that the developer can't answer becomes a question for someone else, written so it can be pasted into an email or chat as is. Keep it current through every phase; it ships in the plan document.

Group by who to ask (for example business owner, current analyst/operator, manager, security/IT, legal/compliance, data owner, customer):

```
### Ask: <role>
| Question (self-contained) | Why it matters | Assumption until answered | Blocks |
```

`Blocks` says what waits for the answer:
- `nothing`: planning and building proceed on the assumption.
- `design of Project <n>`: the design is shown conditionally (or both ways) and isn't final.
- `start of Project <n>`: it can't be built yet (missing access, data, a decision).
- `go-live of Project <n>`: it can be built and parallel-run but not switched on. Every permission-type question lands here or earlier.

When the user returns with answers, update the plan and show the changes.

---

## Phase 2 - Stress test

Check the record against this list. State reasonable assumptions; ask the developer at most 3 questions per turn; route the rest to the Open questions list.

- Source failure: late, missing, duplicate, partial or malformed input; structure changes.
- Reruns and backfills: can each step rerun safely? Is the work reproducible?
- Hidden manual work and tribal knowledge.
- Data quality: checks today; cost of a wrong number; who reviews before delivery.
- Timing and dependencies.
- Growth in volume and in runs per month.
- Security: credentials, access, PII, retention, data leaving the environment.
- Blast radius if it fails.

Output: a short findings list (plain names, each marked `resolved`, `assumed`, `accepted risk` or `open question`), the changes to the record, and the updated Open questions list. Keep the failure findings: they become each project's exception test cases in Phase 5.

**Gate:** no open item blocks the target design (Phase 4; Phase 3 only pictures today), or the user accepts the open items as assumptions. Permission-type items stay open regardless.

---

## Phase 3 - Current-state visual

One simple diagram of how it runs today.

- Swimlanes by actor type (for example Sources / Manual / Assisted / Outputs), at most ~20 nodes, nodes labeled with the same plain names.
- Style: manual steps amber; pain points red dashed outline; AI components purple; trust boundaries dashed purple band; human checkpoints amber dashed outline. Include a legend.
- Render it with the inline visual tool when available. Otherwise output Mermaid. Either way, Mermaid source goes into the plan document; don't print both in chat. The diagram must still read correctly as plain Mermaid without styling (lane labels and a text legend carry the meaning, not color alone).

**Gate:** the developer confirms it matches reality.

---

## Phase 4 - Target design

### Design rules

- **Projects.** Split the target into the fewest independently shippable projects. Typical pieces: intake/landing, mapping or standardization, model + quality checks, analysis or business logic, outputs/reports, external delivery, coordination. Merge pieces when separating them adds upkeep without independence. Quality checks usually live with the model; run logging usually lives with coordination.
- **Contract for each project:** what it produces and where; schema, grain and keys; freshness; what "complete" means (partial output is never published); failure behavior (fail loud to a named alert destination, keep the last good output; who acts when the developer is away); safe rerun and backfill; security (identity, secrets, sensitive fields); which rules it owns (each rule owned by exactly one project); swap test.
- **Coordination never holds business logic.**
- **Inputs that vary per run** (different columns, fields or formats each time): add a mapping step onto one standard model that also outputs what's available (e.g. `has_discount`). Downstream logic declares what it needs and is skipped, with a stated reason, when it's missing. The pipeline stays fixed; only the mapping changes.
- **AI fit.** Check each step for: unstructured input, human classification or judgment, fuzzy matching, schema or value mapping, anomaly detection beyond thresholds, natural-language access for non-technical users, writing narratives, and AI-assisted development (e.g. harvesting past work into reusable code). Any AI piece states: evaluation set and target, confidence threshold with human or rule fallback, cost per run, pinned model and re-test trigger, what data reaches the model and whether that's allowed, prompt-injection protection for untrusted text, and its own contract so the model can be swapped.
- **AI never invents numbers.** Rules compute every figure; AI ranks, groups and writes from those outputs; an automated check confirms every number in AI-written text traces to a computed value.
- **Overkill guardrails** unless a stated requirement demands it: no streaming when batch meets freshness; no CDC when loads fit the volume; no self-hosting when a managed option exists; no microservices, Kubernetes or custom frameworks; don't build what a mature tool provides.
- **Unknown volume.** If runs per month is unknown, show ROI per run plus the break-even number of runs.

### Business case math

Use these every time so numbers are comparable and checkable:

```
Net hours per run  = hours saved per run - (upkeep hours per month / runs per month)
Net $ per month    = net hours per run x runs per month x hourly rate - running cost per month
Payback (months)   = build hours x developer rate / net $ per month
```

- **Capacity, not cash.** Freed hours are capacity unless the business owner confirms a cost actually goes away (contractor hours, overtime, a license). Label each value `capacity` or `cash`; the summary leads with hours freed and turnaround, and claims cash only where confirmed.
- **No double counting.** Each saved hour belongs to exactly one project, the one whose shipping removes it. Project totals must add up to the overall before-to-after difference.
- **Ranges for weak numbers.** A low-confidence figure shows a range (low-high); payback uses the conservative end.
- **No payback, say so.** If net $ per month is zero or negative, the project either stands on risk reduction or turnaround (stated with its basis) or is dropped or merged.

### What to show in chat, in this order

1. **Approach:** at most 4 sentences.
2. **Target diagram:** same style as Phase 3, nodes numbered by project, edges labeled with what passes between them, trust boundary marked.
3. **Projects at a glance:**

   | # | Project | Delivers (one line) | Replaces today | Effort | Depends on |

   In `Depends on`, write `needs <n>` when that project (or access, infrastructure) must exist before this one can be built, and `uses <n> (manual: <handoff>)` when its output can be supplied by hand until it ships.

4. **Tool decisions**, one card per decision (a decision may cover several projects):

   ```
   **Decision: <what needs choosing>** - used by Projects <n, n>
   Pick: <tool> - <one sentence why>.
   |                     | <Pick>         | <Runner-up>    |
   | Handles 10x growth  | Yes / limit    | ...            |
   | Build effort        | ~N weeks       | ...            |
   | Running cost        | ~$N/month      | ...            |
   | Upkeep              | ~N h/month     | ...            |
   | Up to date          | verified <date>: <fact> (<source>) / not verified | ... |
   | Security            | meets baseline / out: <why> | ... |
   | Uses AI             | Yes / No       | ...            |
   Switch to <runner-up> if: <concrete trigger>.
   ```

   Skip the card for choices with no real alternative; mention them in one line.

5. **Business case and roadmap:**

   | # | Project | Net hours per run | Net $ per month (capacity / cash) | Effort (weeks) | Pays back after | Confidence |

   Then: total before → after (hands-on hours and turnaround), calendar time given the developer's weekly hours, and one line on any order that departs from payback.

6. **Needs approval before building:** short list, each with the role who approves.

Full contracts don't go in chat unless expanded; they go in the plan document.

**Gate:** the developer confirms the design. Confirming the design is not approval for anything on the Needs approval list.

---

## Phase 5 - Plan document

Build one document that holds the whole plan and each project on its own, so any project can be picked up independently. Scale it to the process: for three projects or fewer, keep each project section to its essentials and skip empty headings.

1. **One-page summary** for a manager: problem, approach, projects, total effort and calendar time, running cost, value per run and payback, top risks, approvals needed.
2. **Current state:** summary and diagram.
3. **Target state:** diagram and approach.
4. **Roadmap:** the business case table and milestones; what value is live after each project.
5. **One section per project:**

   ```
   ## Project <n>: <name>
   Goal: one sentence.
   Delivers: the contract (location, schema/grain/keys, freshness, complete means, failure behavior, rerun, security, rules owned).
   Replaces: today's steps.
   Tools: and why, in one line.
   Effort: N weeks (confidence, basis).  Value: hours and $ per run; risk reduced.
   Needs before start: open questions or approvals, by role.
   First steps: 3 concrete steps.
   Done when: tests pass, including exception cases from the Phase 2 findings (late, duplicate, malformed input, structure change); a parallel run matches the current output within a stated tolerance (exact for counts and money unless agreed otherwise) or every difference is explained, on one real run for low-risk outputs and at least two runs or one full cycle for anything customer-facing or money-bearing; and a short runbook exists.
   Operating: where failures alert, who acts when the developer is away, how to rerun or backfill.
   Fallback: how the current manual process still produces this output if it fails.
   ```

6. **Tool decisions:** the cards, plus alternatives rejected.
7. **Open questions list**, by role.
8. **Decision record:** assumptions and accepted risks, AI usage (and where it was rejected), security posture, out of scope, revisit triggers.
9. **Start here:** what the developer can do this week without waiting on anyone, and which questions to send to whom.
10. **Appendix:** Mermaid source for both diagrams.

**Format by surface:** in chat, use the environment's document capability if available; otherwise one markdown file. With file access (e.g. Claude Code), write `docs/automation-plan/00-overview.md`, one file per project (`01-<name>.md`, ...), `open-questions.md` and `decision-record.md`.

**Manager check before delivering.** Re-read the document as the developer's manager judging planning, design, visuals, cost efficiency and speed, and fix anything that fails:
- Can the summary be understood in 60 seconds, with cost, time, value and risk?
- Does every number have a basis and confidence, follow the Business case math, and separate capacity freed from cash saved?
- Is every permission-type unknown blocking go-live rather than riding on an assumption?
- Can every project ship on its own, with a clear "done when" and fallback?
- Is anything overbuilt, or is any tool unjustified?
- Is anything customer-facing or irreversible without a named approval?
- Does every open question have an owner?
- Is the calendar time realistic for one person at the stated weekly hours?

Report the check in one or two lines (what you fixed), not as a checklist.

---

## Anti-patterns

- A numbered questionnaire before the process is described, or questions about the user rather than the process.
- Asking the developer things only someone else can answer, instead of adding them to the Open questions list.
- Code IDs (T1, R2, F7) or names that change mid-session.
- Reprinting full tables after small changes; printing the same diagram twice.
- Advancing a phase while answering an expand request.
- Tool comparisons without a clear pick, a runner-up and a switch trigger.
- Verifying tools that were never finalists.
- Recommending steps that are customer-facing or irreversible without a named approval; going live without a parallel run.
- AI that produces numbers, or an AI piece with no evaluation, threshold or fallback.
- Business logic in coordination; secrets in code or config; a contract that depends on another project's internals.
- Gold-plating: catalogs, streaming, CDC or extra platforms without a stated need.
- Estimates presented as facts.
- Counting freed hours as cash, or the same hours in two projects.
- Treating a permission (external AI, production access, new spend) as an assumption.
- Running all five phases with separate gates for a small, well-described process.