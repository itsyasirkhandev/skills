Match response length to task complexity. Simple questions get direct, short answers. Complex tasks get full, detailed responses. Never pad responses with restatements of the question or closing sentences that repeat what you just said.

After any coding task, end with:

- Files changed (list every file touched)
- What was modified (one line per file)
- Follow-up needed

For any task involving architecture decisions, debugging complex issues, or non-trivial features: work through the problem step by step before writing any code. Show your reasoning. Identify where you're uncertain. Then implement.

<!-- antislop:start -->

## antislop

For UI, copy, people, mobile layout, or code comments work, read [antislop](antislop/SKILL.md) (core) and then the skill for the task:

- UI / visual: [antislop-ui](antislop-ui/SKILL.md)
- Copy & text: [antislop-copywriting](antislop-copywriting/SKILL.md)
- People: [antislop-human](antislop-human/SKILL.md)
- Mobile / responsive: [antislop-layoutmobile](antislop-layoutmobile/SKILL.md)
- Code comments: [antislop-code](antislop-code/SKILL.md)

<!-- antislop:end -->

## When working in typescript:

- use pnpm as the package manager unless told otherwise
- when adding a package to a project add it with 'pnpm add' instead of manually editing the package json
- run check/format commands when you're done making code changes. if they don't exist, suggest making them for the project you're in
- avoid explicit return types unless absolutely needed
- 'as any' should be an absolute last resort. always use real type safety. lean on type inference instead of manually writing new types over and over again
- avoid running 'dev' or 'build' commands. if you really need to, ask first

## In general:

- when asking sets of questions, always include numbers so it's easy for me to clearly answer and also use the ask_question tool so I can answer your queries easily.
- **Ask, don't assume.** If something is unclear, ask with ask_question tool before writing a single line. Never make silent assumptions about intent, architecture, or requirements.
- **Simplest solution first.** Always implement the simplest thing that could work. Do not add abstractions or flexibility that weren't explicitly requested.
- **Don't touch unrelated code.** If a file or function is not directly part of the current task, do not modify it, even if you think it could be improved.
- **Flag uncertainty explicitly.** If you are not confident about an approach or technical detail, say so before proceeding. Confidence without certainty causes more damage than admitting a gap.

When developing the frontend, follow strictly the accessibility rules defined in [A11Y.md](https://github.com/fecarrico/A11Y.md/blob/main/docs/en/A11Y.md): [https://github.com/fecarrico/A11Y.md/blob/main/docs/en/A11Y.md](https://github.com/fecarrico/A11Y.md/blob/main/docs/en/A11Y.md)
