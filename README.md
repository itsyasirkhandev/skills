> This README was written by AI.

# skills

The skills I use for agentic coding. Each folder is one skill: a `SKILL.md` with a name and description at the top, plus any reference files or scripts it needs.

[AGENTS.md](AGENTS.md) holds my general instructions for coding agents. Drop it into a project root if you want the same defaults.

## Using a skill

Copy a skill folder into your agent's skills directory. For Claude Code that's `~/.claude/skills/` for every project, or `.claude/skills/` for a single repo. The agent reads each description and loads the skill when a task matches it.

Some skills point to other files in their folder (`references/`, `RECIPES.md`, scripts). Copy the whole folder, not only `SKILL.md`.

## What's in here

UI and design

- [antislop](antislop/) is the core rule set against generic AI-looking UI, copy, and code. Load it alongside one of its sub-skills: [antislop-ui](antislop-ui/), [antislop-copywriting](antislop-copywriting/), [antislop-human](antislop-human/) (accessibility, with a contrast checker script), [antislop-layoutmobile](antislop-layoutmobile/), and [antislop-code](antislop-code/) (code comments).
- [animate](animate/) decides whether something should move, then picks the tool, easing, and duration.
- [refactoring-ui](refactoring-ui/) applies Refactoring UI rules for spacing, typography, color, hierarchy, and shadows. Includes reference guides and starter CSS tokens.
- [responsive-design](responsive-design/) covers mobile-first layouts, breakpoints, and container queries.
- [prototype](prototype/) builds several different versions of a UI with a picker to switch between them. It only runs when you ask for it.

Code

- [effect](effect/) is for Effect v4 in TypeScript. The references cover services, schemas, streams, HTTP, and testing.
- [typescript-best-practices](typescript-best-practices/) has type safety patterns for `.ts` and `.tsx` files.
- [vercel-react-best-practices](vercel-react-best-practices/) is Vercel's React and Next.js performance guide.
- [tdd](tdd/) focuses bug fixes on a failing-before, passing-after regression check when a cheap local test is practical.
- [create-verification-skill](create-verification-skill/) creates a project-local verification skill with launch, health-check, driving, evidence, cleanup, and feature-map instructions.
- [maintain-verification-skill](maintain-verification-skill/) audits an existing verification skill against source code and live app behavior, then opens one PR for proven corrections when needed.

Pull requests

- [file-pr](file-pr/) writes PR titles and descriptions that lead with the problem.
- [babysit-pr](babysit-pr/) watches CI and review bots and keeps fixing until the PR can merge.
- [adversarial-pr](adversarial-pr/) reviews AI-written code independently and sizes the review to the change.

Prompts and writing

- [writing-for-subagents](writing-for-subagents/) is for writing the brief you hand a subagent.
- [prompt-engineer](prompt-engineer/) writes and trims system, task, and tool prompts.
- [unslop](unslop/) strips AI writing patterns from prose. This README went through it.
- [create-tasks](create-tasks/) turns a plan into a `tasks.md`.

Tools

- [playwright-cli](playwright-cli/) drives a browser for testing and automation.
- [tmux](tmux/) works with interactive tmux sessions, with psmux on Windows.

[old/](old/) has retired skills for writing specs and technical design plans. I keep them for reference.
