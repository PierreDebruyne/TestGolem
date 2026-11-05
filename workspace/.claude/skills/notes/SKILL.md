---
name: notes
description: Keeps the working notes in notes/, one file per topic with an index. Use at the end of a task to record hard-won facts, and when a topic's file grows past 150 lines.
---

# Working notes

## What deserves a note

A fact that a later session would otherwise rediscover at a cost: how to build, run and test; a trap of a tool or a
platform, and its way around; where something hard to find lives; why the code does something in an unexpected way.
Not what plan.json, the code, the commits or the product's docs already say, and not the story of the session.

## Where

- One file per topic: `notes/<topic>.md`, its name a few lowercase words joined by dashes, such as `rendering.md`
  or `build-and-test.md`. Add to the file of the topic; start a file when no topic fits.
- Each file stays under 150 lines. Split a topic that grows past that into narrower ones. Remove what became false,
  and merge what repeats.
- `notes/README.md` is the index: one line per file, `- [<file>](<file>): what it holds.` Add the line of each new
  file, and change the line of a file whose content changed.
- A planning task names the topics that each task should read in its `notes`.

## Golem V1's notes

`notes/v1-progress.md`, when it exists, holds the notes of Golem V1, imported as they were. What it says of the
work's state (what is done, under way or still to come) is that of the import: plan.json and history/ hold the
current state, so delete such lines once your task makes them false. When your task touches a topic, move the lines
of that topic from it into the topic's file, and delete them there. Once nothing useful is left, delete the file and
its line of the index.
