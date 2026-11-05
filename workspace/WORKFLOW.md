# Session workflow

You work in a workspace: this folder, which holds the documents of the work. It belongs to a git repository rooted
one folder up, where Golem keeps its records (`../metrics/`): never touch them, and stage only the workspace
(`git add .` from here). The product's code lives in its own git repository, in a subfolder of the workspace; the
other folders are resources for your work. [VISION.md](VISION.md) says which folder is which.

Golem keeps the plan of the work in [plan.json](plan.json) and gives each session one task of it, in its prompt. A
session may be interrupted at any time: what is not committed and pushed may be lost.

## Starting

- Do the task of the prompt, and only that.
- Read the notes that the prompt names. For other topics, read [notes/README.md](notes/README.md), the index of the
  notes.
- Open VISION.md, FEEDBACKS.md, `history/` or the product's docs only when the task needs them.
- Never print whole files or long logs: read the parts you need, and summarize the output of tests (failures only).

## Doing the task

- A task without criteria was imported from Golem V1's documents: first write its `criteria` into plan.json, from its
  title and its `details`, which hold V1's text.
- When the task proves too big for one session, split it into sub-tasks, then do the first one and set it to `done`.
  Split when the work proves too big, not in advance.
- A planning task, of kind `plan`, follows the `plan` skill.
- Delegate to the sub-agents of `.claude/agents/`, which run on a cheaper model and keep your context small: exploring
  the code (where things are, how they work) to `explorer`, and builds and test runs to `checker`, without giving
  them a model or an effort. A sub-agent never sees this conversation: tell it the objective, the answer you expect,
  where to look, and what to leave alone.
- Never ask questions in the conversation: decide, and record the choice.

## Ending

- Build and test.
- Write the hard-won facts into `notes/`, as the `notes` skill says.
- Record significant choices in the product's `docs/decisions.md`, and what is missing in its
  `docs/known-limitations.md`.
- Set your task (after a split, the sub-task that you did) to `done` in plan.json, with its `summary`: what changed
  and where, in a few sentences. A task that should not be done is `dropped`, with the reason as its `summary`.
- Commit and push the product, then the workspace (`git add .` from here), with the same message.

Golem then reviews the task against its criteria. When the review asks for changes, Golem sets the task back to
`in-progress`, and its findings come back in this conversation (in a new one for a sub-task that you split off, or
when this one is lost): fix what is right, say in the `summary` why you leave a finding, then end as above, with the
task set to `done` again.

## plan.json

Golem checks plan.json after each session; a session that leaves it invalid is resumed to repair it. Golem sets
tasks and milestones `in-progress`, closes the parents and milestones whose tasks are all finished, and moves the
finished milestones to `history/`.

- Change only your task, its sub-tasks, the requests that you add and the `waiting_on` of the tasks that they
  block. A planning task also adds the tasks or the milestones that it plans, and may change or remove the planned
  milestones that have not started. Never change an `id`, an `origin`, a `base` or the `reviews`.
- Never change the `criteria` of a task that you work on once it has some: Golem puts them back, and its review
  checks them. When one cannot be met, say why in the `summary`, or make a request when the choice is the owner's.
- A milestone has `id`, `title`, `status`, `scope`, an optional `feedback` (the feedback items that it covers) and
  `after`, and `tasks`.
- A task has `id`, `title`, `status`, `kind` and `criteria`, and may have `details` (what to do, beyond the title),
  `notes` (the topics of `notes/` to read first), `after`, `waiting_on`, `summary` and `tasks` (its sub-tasks, with
  the same fields).
- `status` is `planned`, `in-progress`, `done` or `dropped`. A done task has a `summary`, and a task or milestone
  that is done or dropped has no open task under it.
- `kind` is `plan`, `design`, `code`, `routine` (mechanical changes, small fixes) or `docs`.
- `criteria` are what a reviewer checks by reading the product: the behaviour, the tests that show it, the docs.
  Every open task has some, except an imported one.
- An ID is made of letters and digits, with single `.`, `_` or `-` between them. It is unique in the plan and in
  `history/`, letter case aside. The sub-tasks of `M3.2` are `M3.2.1`, `M3.2.2`, and so on.
- `after` names the tasks or milestones to finish first, never a parent or a sub-task; `waiting_on` names requests.
- `feedback`, `criteria`, `notes`, `after` and `waiting_on` are lists of texts. A note topic is the name of its file
  in `notes/`, without `.md`.

A task split in two, its first sub-task done (Golem's `base` left out):

```json
{
  "id": "M3.2",
  "title": "Renderer statistics",
  "status": "in-progress",
  "kind": "code",
  "criteria": ["Draw calls and triangles per frame, with tests", "GPU time per pass, skipped without timestamps"],
  "tasks": [
    {
      "id": "M3.2.1",
      "title": "Draw calls and triangles",
      "status": "done",
      "kind": "code",
      "criteria": ["Draw calls and triangles per frame, counted by the renderer, with tests"],
      "summary": "Added DrawStatistics to the renderer and 6 tests."
    },
    {
      "id": "M3.2.2",
      "title": "GPU time per pass",
      "status": "planned",
      "kind": "code",
      "criteria": ["GPU time per pass from timestamp queries, skipped without them, with tests"]
    }
  ]
}
```

## When the owner is needed

- A request is only for what no session can do or decide: an access, a subscription, a server, or a choice about
  the product's goals that VISION.md leaves open. It is never for a design choice.
- Add it to the `requests` of plan.json: `{"id": "R1", "title": "...", "request": "what you need, and why",
  "status": "open"}`. Put its ID in the `waiting_on` of the tasks that it blocks, then commit, push and end. Golem
  goes on with other work, and gives the task back with the owner's answer, `answers/<id>.md`.
- Never edit WORKFLOW.md, CLAUDE.md, the skills and sub-agents that Golem puts in `.claude/`, VISION.md,
  FEEDBACKS.md, `answers/` or `history/`: Golem and the owner write them. A problem with them becomes a request.
