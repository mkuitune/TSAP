# TSAP

**Tracked System Architecture Plan.** A high-level, medium-horizon, goal-oriented code factory management system with simple rules.

Once you've figured out what you want to build over the next days and weeks, track it in TSAP.

Give [`tracked-system-architecture-plan-v1.5.md`](tracked-system-architecture-plan-v1.5.md) to your LLM assistant. Decide together what you want to build, what counts as done, and how to track progress.

This is not supposed to replace your architecture spec or other planning documents. Its purpose is to provide an unambiguous control surface for project management.

TSAP has just three building blocks:

### 1. System Milestones

`S1`, `S2`, `S3`, … describe useful successive states of the whole system. Each milestone says what must be achieved, not which tickets or pull requests must be written.

Milestones are the top-level view of progress. Ask: **“What remains before S3?”**

### 2. Tracks

Divide the system into engineering concerns that can be developed largely independently: for example, **Testing** (`TE`), **Automation** (`AU`), and **Renderer** (`RE`). Each track gets a name and a unique short prefix.

### 3. Track Levels

Within each track, define successive, verifiable outcomes: `TE1`, `TE2`, `AU1`, `AU2`, … A level can describe a new feature, a quality target, a refactoring, or a configuration. Give each level a clear definition of done.

Levels are cumulative: reaching `TE2` means the outcomes of `TE1` still hold.

## A tiny example

| System Milestone | Testing | Automation | Outcome |
|---|---|---|---|
| **S1** | `TE1` | — | Local unit tests with a coverage target |
| **S2** | `TE2` | `AU2` | Reference integration tests and hosted PR checks |
| **S3** | | `AU3` | CI checks required before merging |

`TE2` includes `TE1`; `AU2` includes `AU1`. An empty cell inherits the previous milestone's level. `—` means the track has no required level yet.

The milestone table is the plan's control surface. The track-level definitions provide the detail behind each cell.

## Working with an LLM

Start with a rough goal, propose tracks, and agree on the milestones. Then define the track levels and their acceptance criteria. Both you and the LLM can propose changes as the project develops.

Keep the plan separate from implementation tasks: one PR may contribute to several levels, and a level may take many PRs. Record evidence of completion—tests, measurements, and PR links—without mistaking a merged PR for a finished level.

A TSAP can be Markdown, HTML, or any format that keeps the milestone matrix, level definitions, and status easy to read.

**[Read the TSAP guide and authoring specification](tracked-system-architecture-plan-v1.5.md)** for the complete rules, document structure, and formal definition.
