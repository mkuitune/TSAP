# Tracked System Architecture Plan (TSAP)

**Guide and authoring specification · Version 1.5**

## 1. Purpose

A **Tracked System Architecture Plan (TSAP)** helps engineers and LLMs plan and track long-horizon software and systems development. It divides a system into **Tracks**—coherent engineering concerns such as Testing, Native Build, CI Automation, or Renderer. Each track advances through named, verifiable **Track Levels**. **System Milestones** specify which levels the system must reach across tracks.

The plan divides development into **System Milestones** (`S1`, `S2`, …), each with defined content. It answers questions such as *“What needs to be done to reach S3?”* and *“What remains?”* without prescribing how work is divided into tickets or pull requests. Milestone descriptions show how the system evolves. Technical architecture and operations guides can accompany the plan.

See [Section 6.2](#62-system-milestones) for an example System Milestones table.

## 2. Vocabulary

| Term | Meaning |
|---|---|
| **System** | The software or engineering system being developed. |
| **Track** | A named concern that can be planned largely independently of other concerns. Give each a short name and stable prefix (Testing, `TE`; Automation, `AU`; Native Build, `BN`). |
| **Track Level** | A numbered, named, verifiable outcome within a track. It may concern behavior, quality, internal structure, tooling, or configuration. For example, `TE2`. |
| **System Milestone** | A project-wide target that identifies the track levels required for a useful system state. For example, `S2`. |
| **Status and evidence** | What has actually been implemented, what has been verified, and what is still missing. |

**Track Levels are the units of planning.** A level may require several changes and acceptance criteria, and may be delivered through one or many PRs.

## 3. How to make a plan

1. **Define the system and tracks.** Choose boundaries by separately developed concern. Name each track and give it a unique prefix.
2. **Define track levels.** Number them in their intended progression (`TE1`, `TE2`, …). Give each a concise outcome, an unambiguous definition of done, and—where useful—prerequisites or qualifications in prose. A level may represent an additive check or a replacement of an older technical arrangement. If the ordering of some levels is hard to determine, consider separating them into tracks—for example, Unit Testing and Integration Testing rather than Testing.
3. **Define system milestones.** For `S1`, `S2`, … choose the track levels needed at each stage and describe the resulting system in ordinary language. Milestones coordinate the tracks; they need not advance every track. System Milestones are the primary way to plan and track overall progress.
4. **Record actual progress.** Distinguish specified targets from implementation and verification. Attach links to issues, PRs, runs, measurements, and decisions as supporting information, not as the definitions of the levels.
5. **Revise the plan as needed.** Preserve stable identifiers and explain changes to a level's contract. Keep milestone descriptions and the status record consistent with the level definitions.

A human can start with only the track names and desired milestone outcomes. Both the user and LLM can propose track levels, negotiate definitions of done, assemble the milestone matrix, and generate the document. Unknown design decisions should be marked as open, not silently filled in.

## 4. Planning rules

- **Progression within a track.** For example, consider a Testing track with the prefix `TE`. Reaching level `TE3` means levels `TE1` and `TE2` have also been satisfied, or their intended guarantees are still provided by a documented successor. Later levels may change implementation details; they must not silently discard earlier agreed outcomes.
- **Multiple changes per level.** One level can require several related checks, configurations, or modifications. Its acceptance criteria define the whole outcome.
- **Milestones accumulate.** An unchanged track retains its previously reached level. Later milestones cannot silently reduce requirements established by earlier milestones.
- **Readable milestone matrix.** Each cell names the newly required level(s) in that track. `—` means no level is required yet; a blank cell inherits the preceding milestone's level. Repeating a level is allowed when adding a qualification, such as verifying an existing suite on another platform; state the qualification explicitly.
- **Completion is evidence-based.** “Implemented,” “verified,” and “milestone achieved” are distinct claims. A merged PR, a passing subset of tests, or an optimistic note is not itself evidence that all criteria are met.
- **Prerequisites.** State important technical prerequisites in the relevant level definition.
- **Tickets and PRs are orthogonal.** A PR may contribute to several track levels; a level may require many PRs. Link related tickets and PRs for traceability.
- **Baseline levels are allowed.** Levels such as `BN1` may document an existing starting state; indicate whether a level is a baseline, an improvement, or a planned target. Record evidence for baseline claims as for other levels.

## 5. TSAP document format

A TSAP may be written in Markdown, HTML, or another format that preserves its tables, level definitions, and evidence. The following structure applies regardless of format.

### 5.1 Document structure

Recommended sections, in reader order:

| Section | What the reader should find |
|---|---|
| **Overview / status** | Scope, latest observed state, date or revision, major unresolved gaps. |
| **Planning model** | One paragraph defining tracks, levels, milestone inheritance, and completion. |
| **Tracks** | Prefix, name, responsibility, and baseline/level convention where relevant. |
| **System milestones** | **Main matrix:** rows `S1`–`Sn`, columns by track, a concise system outcome, and status. |
| **Track levels** | One index/table with every ID, definition of done, intended milestone, and noteworthy prerequisites. |
| **Principles and decisions** | Architectural constraints and rationale that affect multiple levels. |
| **Milestone details and evidence** | Implementation notes with references to relevant PRs, acceptance checks, diagrams, validation runs, known limitations, and remaining gaps. |

**Keep the matrix and level index close to the top.** A reader should be able to answer “What is S2?” from the milestone row and its referenced level definitions.

### 5.2 HTML implementation

For a standalone HTML version, use embedded CSS, working section links, readable tables, and no required JavaScript or external assets.

- Use stable anchors for each track, level, milestone, and major decision; link matrix cells to level definitions.
- Put concrete completion criteria in the level definitions. A level can link to a longer explanation of implementation or rationale.
- Display status as text, not just color. Make wide tables scrollable and the page printable.
- Separate **target**, **observed result**, and **limitation**. Date or otherwise qualify observations that may become stale.
- Do not manufacture test counts, build success, platform parity, ticket references, or verified completion.
- Keep historical investigation and measurements accessible, but don't let them obscure the normative level definitions.

### 5.3 Instructions for an LLM author or editor

1. Read the user's scope, track names, desired milestones, and any existing plan. Preserve the source's distinctions and IDs.
2. Propose only missing levels and criteria. Treat suggestions as proposals until agreed; don't infer unstated guarantees from conventional industry practice.
3. Expand milestone inheritance internally and check that each row references defined levels and does not revoke previously required outcomes.
4. Build the **milestone matrix first**, then the level index, and finally the supporting prose and diagrams. Ensure the stated milestone outcome follows from its levels.
5. Report the difference between planned and demonstrated completion explicitly. If a level has several criteria, incomplete criteria keep it incomplete.
6. On edits, update all affected matrix cells, level definitions, descriptions, and evidence without unnecessarily renumbering unrelated IDs.

## 6. Minimal worked example

Two tracks: **Testing** (`TE`, two levels) and **Automation** (`AU`, three levels). Three system milestones combine them.

### 6.1 Tracks and levels

| Track level | Definition / acceptance criteria |
|---|---|
| **TE1 — Unit testing foundation** | Unit tests run locally, return a failing exit code on failure, **and achieve at least 90% statement coverage** within a documented scope. The documented coverage command demonstrates this. |
| **TE2 — Reference integration tests** | A repeatable suite processes committed reference inputs and evaluates outputs using documented comparison rules. TE1 remains satisfied. |
| **AU1 — Local check command** | One documented command invokes the project's agreed local checks and propagates failures. |
| **AU2 — Hosted CI** | A hosted workflow runs checks on PRs and reports their results; AU1 remains available locally. |
| **AU3 — Merge gating** | Branch protection requires the designated CI checks before merging; AU1 and AU2 remain operational. |

`TE1` groups related acceptance criteria into one verifiable outcome.

### 6.2 System milestones

| Milestone | Testing | Automation | System outcome |
|---|---|---|---|
| **S1** | `TE1` | — | Unit tests run locally with measured 90% coverage. |
| **S2** | `TE2` | `AU2` | Reference integration tests exist; one local check command and hosted PR checks are available. |
| **S3** |  | `AU3` | Earlier verification remains; designated CI checks now gate merges. |

**Legend:** `—` = no level yet; **blank** = inherit. Although `AU1` is not named in S2, requiring `AU2` includes it. S3 inherits `TE2` and `AU2` while adding `AU3`. These are planned outcomes, not assertions about a real repository.

Milestone cells reference the level definitions directly. Each higher track level includes the outcomes of earlier levels.

---

## Appendix A. Formal definition

### A.1 Tracks and levels

Let \(T\) be a finite set of tracks. Each \(t\in T\) has ordered levels \(L_t=(\ell_t^1,\ldots,\ell_t^{n_t})\), each with a stable ID and a natural-language completion predicate \(P_t^i(x)\) over observed system state \(x\). Predicates may have multiple acceptance criteria and cover behavior, quality, structure, configuration, or evidence.

The effective requirement at level \(k\) is cumulative:

\[
Q_t^k(x)=\bigwedge_{i=1}^{k}P_t^i(x),\qquad Q_t^0(x)=\mathrm{true}.
\]

Thus \(Q_t^{k+1}(x)\Rightarrow Q_t^k(x)\). A later level may replace an implementation but must preserve earlier agreed outcomes, or explicitly revise their contracts. Level order is unrelated to tickets or PRs.

### A.2 System milestones

Each milestone \(S^m\) defines target levels \(s_m(t)\in\{0,\ldots,n_t\}\) for all \(t\in T\). In its matrix:

- Level ID: set that track's target.
- Blank: inherit the preceding milestone's target; initially 0.
- `—`: no level required yet (level 0).
- Repeated ID with a qualification: retain the level and add the stated acceptance condition.

Milestones progress monotonically:

\[
\forall t\in T:\quad s_m(t)\ge s_{m-1}(t).
\]

Achievement requires all implied levels **and** explicit milestone-specific conditions:

\[
\operatorname{Achieved}(S^m,x)\iff
\left(\bigwedge_{t\in T}Q_t^{s_m(t)}(x)\right)
\land R_m(x),
\]

where \(R_m(x)\) is the conjunction of any additional milestone conditions, or `true` if none. Monotonicity applies to a single milestone sequence; alternative plans may differ.

### A.3 Tracking semantics

Specifications are distinct from implementation and evidence. A merged PR does not establish \(P_t^i(x)\); verification must support the claim. Evidence can be qualified by platform, toolchain, data, or date, and regressions may invalidate prior achievement. Status labels are reporting conventions.

Technical prerequisites may be stated in level descriptions; milestone membership specifies required outcomes, not dependency direction. Tickets, commits, and PRs relate many-to-many to levels.

### A.4 Example expansion

For Section 6:

\[
 s_1=(TE1,AU0),\quad s_2=(TE2,AU2),\quad s_3=(TE2,AU3).
\]

Therefore S1 requires TE1; S2 requires TE1, TE2, AU1, AU2; S3 additionally requires AU3. Blank matrix cells do not remove inherited requirements.
