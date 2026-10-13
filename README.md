# skills

A document-driven workflow for coding agents, packaged as agent skills.

Each skill is one `SKILL.md` that an agent loads when its description
matches the task. Together they take a feature from idea to retired docs:

**shape → design → plan or roadmap → review loop → finalize → execute →
accept → hand off or close**, plus a few git helpers (commit, merge, fix).

The skills share a few convictions:

- **The user decides.** Agents recommend; product decisions and anything
  expensive to reverse go to the user as one plain question each, with a
  recommendation first.
- **Never block.** When the user is away, an agent records the question —
  context, options, recommendation, what it did meanwhile — takes the
  reversible path, and keeps going.
- **The shape comes before the mechanism.** Every plan opens with one
  sentence a person would say and a ladder of use cases, smallest first;
  milestones follow the ladder, not the layers.
- **The tree is the truth.** Documents are claims; the code and `git log` are
  checked before anything is called done.
- **Persisted schema or format changes are announced** ("this changes the
  database schema"), approved, and first run against disposable data.

They assume nothing about your project beyond git. Where a skill mentions
your principles, doc map, product status page or decision records, it means
"if the project has them".

## The skills

| Skill | What it does |
|---|---|
| [draft-experience](skills/draft-experience/SKILL.md) | Shape a feature as a product — the sentence, the ladder, what stays hidden, the open questions — into `EXPERIENCE.rev1.md`. |
| [draft-technical](skills/draft-technical/SKILL.md) | Design how it is built as a conversation with the user; write `TECHNICAL.rev1.md` only when the user says the design is ready. |
| [build-feature](skills/build-feature/SKILL.md) | Shape, plan (`PLAN.rev1.md` → review → final) and code a single feature yourself. |
| [draft-roadmap](skills/draft-roadmap/SKILL.md) | Plan a large feature as `ROADMAP.rev1.md`: milestones that follow the ladder, plans small enough to delegate. |
| [review-doc](skills/review-doc/SKILL.md) | Review any rev with fresh eyes into `<STEM>.revN.notes.md`: product shape first, then reality, principles, migration, complexity. |
| [review-doc-loop](skills/review-doc-loop/SKILL.md) | The reviewer's side of the review loop, run without the user relaying each round. |
| [answer-notes-loop](skills/answer-notes-loop/SKILL.md) | The author's side of the review loop: answer each notes file with the next rev, until ready, capped or blocked. |
| [finalize-doc](skills/finalize-doc/SKILL.md) | Turn the signed-off rev into `<STEM>.final.md` with the collapsed history of decisions, then delete revs and notes. |
| [execute-plan](skills/execute-plan/SKILL.md) | Build a `PLAN.final.md` yourself, phase by phase, one verified commit per phase, questions recorded instead of blocking. |
| [execute-roadmap](skills/execute-roadmap/SKILL.md) | Deliver a `ROADMAP.final.md` as a lead: one sub-agent per plan (isolated checkouts where possible), review, verify, merge, commit. |
| [write-acceptance](skills/write-acceptance/SKILL.md) | Write `ACCEPTANCE.md`: the ordered checklist of what only a person can verify, each step with its observable. |
| [write-handoff](skills/write-handoff/SKILL.md) | Write `HANDOFF.md` so a fresh session can pick up unfinished work: tree state, where to resume, the traps. |
| [resume-handoff](skills/resume-handoff/SKILL.md) | Read a handoff, verify it against the tree, propose merge and scope, wait for the user, then continue. |
| [close-plan](skills/close-plan/SKILL.md) | Retire a finished plan: review acceptance, triage every phase, fold landed work into existing pages, carry the rest to a V2 rev1. |
| [close-roadmap](skills/close-roadmap/SKILL.md) | The same for a roadmap: decision records, architecture and product docs, a folder README, `ROADMAP-V2.rev1.md`. |
| [commit-changes](skills/commit-changes/SKILL.md) | Write a commit message sized to the change, for `git log` and for changelog writers; show it, commit on approval, never push. |
| [merge-branch](skills/merge-branch/SKILL.md) | Merge as a review gate: survey, check against principles, ask about product-shaping conflicts, choose the direction. |
| [fix-bug](skills/fix-bug/SKILL.md) | Fix a bug or make a small improvement: root cause, justified solution, and the simplification the bug points at. |

### How they chain

```
draft-experience ──► draft-technical ──► build-feature ──► execute-plan ──► close-plan
     (optional)                     └──► draft-roadmap ──► execute-roadmap ──► close-roadmap

every rev:   draft-* / build-feature ⇄ review-doc   (or answer-notes-loop ⇄ review-doc-loop)
sign-off:    finalize-doc  →  <STEM>.final.md is what gets executed
end of run:  write-acceptance (finished work)  ·  write-handoff (work still owed)
next session: resume-handoff → execute-plan / execute-roadmap
throughout:  commit-changes · merge-branch · fix-bug
```

A small change that touches one area, adds no persistent state, no external
service and no new surface goes straight to `fix-bug`.

## The documents

Planning documents live in the project's planning folder — `docs/<feature>/`
unless the project uses another. Each document has a **stem** and moves
through revisions:

| File | Written by | What it is |
|---|---|---|
| `notes/*` | the user, or a product conversation | The intent. Never deleted; marked historical at close. |
| `EXPERIENCE.revN.md` | draft-experience | The product shape: sentence, ladder, rung one in full, what stays hidden. |
| `TECHNICAL.revN.md` | draft-technical | The design and the reasons for it; once final, it owns the design and plans cite it. |
| `PLAN.revN.md` | build-feature | A single feature's increments and their verification. |
| `ROADMAP.revN.md` | draft-roadmap | Milestones and delegatable plans, with what can run in parallel. |
| `<STEM>.revN.notes.md` | review-doc | Review notes on one rev; the first line is the verdict. |
| `<STEM>.final.md` | finalize-doc | The signed-off rev plus the collapsed history. The only executable document. |
| `EXECUTION.md` | execute-plan / execute-roadmap | What landed (with hashes), what was deferred, what was and was not verified. |
| `ACCEPTANCE.md` | write-acceptance | The checklist a person works through to accept the work. |
| `HANDOFF.md` | write-handoff | The briefing for the next session; consumed and deleted by resume-handoff. |
| `<STEM>-V2.rev1.md` | close-plan / close-roadmap | What is still owed, re-entering the review cycle. |

**Open questions** go wherever the project keeps them. If it has no
convention, use a questions section or file beside the plan. Each entry
carries the context, the options, a recommendation, and what was done
meanwhile; one decision per entry.

Each rev and its notes are committed when the round closes, so git keeps the
reasoning; `finalize-doc` and the close skills then delete the superseded
files rather than archiving them.

## Naming

Every skill is named `<verb>-<object>[-<qualifier>]`, and the folder name and
the `name:` in its front matter are the same.

- **Verb** comes first, in the imperative, from this list:

  | Verb | Means |
  |---|---|
  | `draft` | write the rev1 of a document that goes through review |
  | `review` / `answer` | the reviewer's and the author's side of a review round |
  | `finalize` | turn a signed-off rev into `<STEM>.final.md` |
  | `execute` | build from a final document |
  | `build` | plan and build in one skill |
  | `write` | write a document that is not reviewed |
  | `resume` / `close` | pick up unfinished work, or retire finished work |
  | `commit` / `merge` / `fix` | git and code work |

- **Object** is the document stem the skill writes or reads, lowercase:
  `experience`, `technical`, `plan`, `roadmap`, `handoff`, `acceptance`.
  `doc` means any stem. Git skills name what they act on (`branch`,
  `changes`).
- **Size** is said only by `plan` (one person) versus `roadmap` (parallel
  agents), never by adjectives like `large`, `small` or `new`.
- **Qualifier** only names a mode or a target: `-loop` runs without the user
  relaying each round.

## Install

With the [skills CLI](https://skills.sh):

```sh
npx skills add koruworks/skills
```

Or by hand: copy `skills/<name>/` into your agent's skills directory — for
example `.claude/skills/` for Claude Code, or `.agents/skills/` for agents
that read that path. Copy the whole set if you use the workflow; the skills
link to each other by relative path.

## License

MIT — see [LICENSE](LICENSE). Copyright (c) 2026 Koru Works, Inc.

You can use, adapt and redistribute these skills, including inside
proprietary projects, without any copyleft obligation. Redistributed copies
or substantial portions must keep the copyright and licence notice.
