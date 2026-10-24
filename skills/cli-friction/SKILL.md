---
name: cli-friction
description: Trial a command-line tool through its help alone — do the task the user gives using only the tool's --help, output and errors, never its source or repo files, and keep a friction log of every place the help made you guess, retry or stall, ending in a friction report and ticket drafts that would fix the help. Use when the user wants to know whether an agent can work a CLI from its help, to dogfood or blind-test a CLI's help, or to find where the help leaves an agent stuck (只看 help 用这个 CLI、测 help 够不够用、找 help 的摩擦). Not for building or changing the CLI itself (agent-cli).
---

# cli-friction

An agent meets a tool in the field with the binary and its help, nothing else. You play that agent. The task is the user's; your product is the **friction log**: every place the help made you guess, retry, or stall. A task done smoothly is a fine result; a task done by guessing is friction, even when the guess was right, because the next agent may guess wrong.

## The rule: help only

For the whole run, ticket drafts included, everything you learn about the tool comes from running it:

- `<cli> --help`, `<cli> <command> --help`, `<cli> help <topic>`, and whatever any command prints: its output, its errors, its exit code.
- What the tool manages (its config, its state, the repo it works on) you learn by asking the tool, not by opening the files yourself.
- Files the user's task hands you (an input file, a directory to work in) you may read as the task needs.

The tool's source, README, docs, tests, issues, its installed SKILL.md and the web stay closed. The pull to open one is itself friction: log what you wanted to know, then carry on from the help or stop.

Knowledge you already carry about the tool (from this conversation, CLAUDE.md, memory) enters the trial only through the log: write the entry ("help didn't say X; I knew it from CLAUDE.md"), then use it. If you read the tool's source earlier in this conversation, say so at the top of the report: the trial is contaminated, and a fresh session gives a clean one.

## Steps

1. **Pin the trial.** Run `<cli> --version`; copy the user's task verbatim. If the task would change shared state (push, file issues, delete, send), tell the user before the first write and wait for a go-ahead; a tool built for agents names a command that says what would happen, so try that first. Done when version and verbatim task head your notes.
2. **Do the task.** Start from `<cli> --help`. Write each friction entry the moment it happens, not from memory at the end. Done when the task is finished, or you are stuck with no next step the help can give.
3. **Check the outcome.** Did you do what the user asked? Show it from the tool's own output. If it cannot be shown, say "not verified".
4. **Write the report** (format below), entries ordered by cost.
5. **Draft tickets.** One ticket per distinct fix (merge entries that share one). Send them to the repo the tool is released from (its help or `brew info <cli>` names it; ask when unsure). The one opening past the rule: that repo's issue conventions and its open issues, read so the drafts follow its conventions and repeat no ticket. Each ticket carries the evidence and the help text you propose; where that text lives in the code is for whoever implements it to find. Show the full drafts; file only after the user confirms.

## A friction entry

| Field | What it holds |
|---|---|
| needed | what you wanted to know or do |
| looked | which help you read (command, topic), quoting the closest line it gave |
| did | guessed · trial and error (n tries) · outside knowledge · stuck |
| evidence | the command you ran and the part of its output that matters |
| fix | the sentence the help should say, and where (which command's help, or a topic) |
| cost | blocked · wrong result · wasted steps · guessed right |

Name each entry by its kind:

- **missing**: the help never says it.
- **buried**: it says it, but not where you looked.
- **ambiguous**: it says it in a way that reads two ways.
- **wrong**: the tool did something other than the help says.
- **bare error**: an error without the line that fixes it.
- **opaque output**: a key or value whose meaning the help does not give.

## Report

```markdown
# <cli> <version> — friction report

Task: <verbatim>
Outcome: done | partly | not done — <evidence>
Commands run: N (help reads H, errors E)

## Friction
1. [<cost>] <kind> — <needed>
   - looked: …
   - did: …
   - evidence: …
   - fix: …

## What worked
- <help that answered first time, worth keeping>
```

Write the report in the user's language; keep commands, output and proposed help text as they are.
