---
name: plan
description: Plans a milestone of plan.json into tasks, or plans the next milestones. Use in a task of kind plan.
---

# Planning

WORKFLOW.md's "plan.json" section gives the fields. Your task's criteria say what to plan; then end as WORKFLOW.md
says, the task `done` with a summary that names what you planned. Keep your task in the plan, with its `id`, `kind`
and `title`: never replace it with the tasks that you plan, which come after it.

## Read first

- The milestone's `scope` and `feedback`, and what they point to in VISION.md and FEEDBACKS.md.
- Your task's `details`: when the plan was imported, they hold Golem V1's last "Next step", which often proposes the
  tasks.
- The product's `docs/known-limitations.md`.
- `history/README.md`, and the `history/<id>.json` of the milestones that the work builds on.
- The code only as far as the plan needs it.

## A milestone

Add the milestone's tasks after yours, in the order of the work. Each has:
- an ID after yours: `M13.2`, `M13.3`, and so on;
- a `title`, and `details` when the title is not enough;
- the `status` `planned`;
- a `kind`: `design` for choices to make and write down before the code, `code`, `routine` for mechanical changes
  and small fixes, `docs`, or `plan`. Golem runs `routine` and `docs` on a cheaper model or effort, and the other
  kinds on the strongest: give `routine` and `docs` only to work that needs little judgement;
- `criteria`, each a short sentence that a reviewer can check by reading the product: the behaviour, the tests
  that show it, the docs that change. Not "works well" or "is clean";
- `notes`: the topics of `notes/` to read first, the names of their files without `.md`;
- `after`, only where the order matters: a task that needs the code or the design of another.

Size each task for one session that ends well before the context fills: one area of the product, leaving it built
and tested. Do not plan sub-tasks: a session splits its task when the work proves too big.

The last task closes the milestone, of kind `code`: a review of the whole milestone against its scope, the fixes it
finds, the docs, and the product's `docs/known-limitations.md` for what is left.

## The next milestones

In the milestone "Plan the next milestones":
- Read VISION.md, FEEDBACKS.md, the product's `docs/known-limitations.md` and `history/README.md`.
- Add 1 to 9 milestones after this one, in the order of the work. Each has an ID after the highest `M<n>` of the plan
  and of `history/`, a `title`, a `scope` that says what it covers and when it is done, the `feedback` items of
  FEEDBACKS.md that it covers, the `status` `planned`, and `"tasks": []`: each is planned when it starts.
- End the plan with a new milestone of the title "Plan the next milestones", the `status` `planned`, the scope
  "Plan the next milestones from VISION.md, FEEDBACKS.md, the product's docs/known-limitations.md and
  history/README.md.", and `"tasks": []`.
- Planned milestones that have not started may be changed or removed.
