# AI Task Manager

> 🦊 **Source, issues, and releases live on [GitLab](https://gitlab.com/arturnmk/ai-task-manager).** This page is a showcase.

Local-first board app for `ai-agent-workflow` markdown tasks.

AI Task Manager reads task files directly from a project folder and gives you a desktop or browser board over `backlog`, `inbox`, and `done`. It is built for AI-first workflows where agents such as Claude, Codex, Gemini, Copilot, or a human operator share the same markdown tickets instead of using a separate SaaS tracker.

Not Jira. Not Trello. Not whatever ticket tool drained the joy out of your last job. This is a command center for AI-first work — your agents create and execute, you set scope and stay oriented. Kanban-flavoured because the vocabulary is useful; the resemblance ends there.

![AI Task Manager demo](./img/demo.png)

## Why This Exists

Starting a project with AI is easy. Day one is great — you have an agent, a fresh idea, and momentum, and the first week feels like cheating.

Day thirty is a different story. You've been talking to Claude, Codex, Gemini, and yourself for weeks, and there are decisions in the codebase you can't quite trace back to anyone (although you suspect it was you, last Wednesday, around 1am). Half the work-in-progress lives in chat windows you've already closed, while the planning sits in your head where it helps nobody, including future-you.

I built this board for that Tuesday. When you point it at a project folder, it reads the same markdown files the agents are already touching, so you get a thing to actually look at: three columns, real cards, a detail pane. Your filesystem is the database, since the app's whole job is to render what's already in there. Uninstall the board tomorrow and your tickets will still be sitting in the folder, exactly the way the agent left them.

## How To Use It

This part matters more than the UI: **work in tickets, not chat.**

- **Tell the agent to log findings as tickets.** Every bug it spots, every refactor it considers, every missing feature, every security finding — these become files in `ai-agent-workflow/`, where the board can surface them and next week's agent can find them again. Yes, even the small stuff. Especially the small stuff, actually.
- **Let the agent own ticket creation.** You *can* write tickets by hand, and sometimes I do, although the workflow is designed so that the agent does it. That way the friction of good ticket hygiene falls on the AI's shoulders, which is what those shoulders are there for.
- **Structure keeps the agent on rails — in both directions.** Every ticket has Problem, Required Changes, Acceptance Criteria, Notes, plus required metadata (severity, scope, assignee, status). *Going in*: the AI has to actually *think* before proposing anything, because vagueness gets pretty obvious once you have to write Acceptance Criteria for "fix the auth thing". *Coming back out*: when the same agent — or a different one tomorrow — picks up "B3. fix preview crash on large PDFs", the Problem is already stated, the Required Changes are listed, and the Acceptance Criteria are spelled out. Far less room for creative reinterpretation than "remember the auth thing we discussed last week".
- **You delegate via the `Assignee` field.** By default, the agent creating a ticket assigns it to itself, since it has the context and you haven't told it to do anything else. When you want a specific agent on a specific job — "give B2 to Gemini", "have Qwen pick up the EASY ones", "leave B3 for Marcus" — change the Assignee and the next session honors it. Any name works; the well-known agents (Claude, Codex, Gemini, Copilot, Human) get tone-styled badges, anything else (a local model, a teammate's first name, a fictional cat) renders neutral. The one hard rule: anything needing credentials, secrets, or production deploy access auto-routes to `Human`, because agents should not be holding your keys.

When you open the app on a project that doesn't yet have `ai-agent-workflow/`, you'll see a copy-pasteable prompt. Hand it to your agent, and the whole structure scaffolds itself.

## Autonomous Loops

The `inbox/` vs `backlog/` split is also the contract for set-and-forget agent runs. A scheduled agent (a cron job, a watchdog, a Codex Automation, an `@autonomous` skill, a `git push`-triggered CI agent — pick your flavour) can safely chew through `inbox/`, work the tickets, and move them to `done/` while you sleep. `backlog/` stays as the human-curated next-up list it shouldn't touch.

So the loop you build can be as small as:

1. Read `ai-agent-workflow/inbox/`.
2. Pick the highest-severity ticket assigned to me (or unassigned).
3. Do the work the ticket specifies.
4. Write verification notes under `## Notes`, flip `Status` to `DONE`, move to `done/`.
5. Sleep until next run.

The board you've been looking at is the same board the loop sees. Open the desktop app at any time to inspect what the autonomous agent has been up to, redirect with the Assignee field, or move tickets between columns. The files are the source of truth either way.

## Features

- Multi-project board picker
- Three workflow columns: `backlog`, `inbox`, `done`
- Markdown detail view
- Filters for assignee, scope, severity, and status
- Drag/drop or metadata-driven status changes
- Tauri desktop app with a web fallback for development

## Get It

- **macOS desktop app:** [GitLab releases](https://gitlab.com/arturnmk/ai-task-manager/-/releases)
- **Build from source:** [setup on GitLab](https://gitlab.com/arturnmk/ai-task-manager#build-from-source)
- **The reusable agent skill (`ai-agent-workflow`):** [SKILL.md on GitLab](https://gitlab.com/arturnmk/ai-task-manager/-/blob/main/SKILL.md)
- **Bugs & questions:** [Issues on GitLab](https://gitlab.com/arturnmk/ai-task-manager/-/issues) — I check there, not here

## License

MIT with an Attribution Addendum. See [LICENSE on GitLab](https://gitlab.com/arturnmk/ai-task-manager/-/blob/main/LICENSE).

Free to use, modify, redistribute, and ship commercially. The one extra requirement: any fork or derivative that exposes a UI or documentation must keep a visible "Originally created by Technikatsu" attribution.
