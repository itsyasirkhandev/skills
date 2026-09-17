---
name: create-spec
description: spec for major feature or change. use when user ask to create the specification document (spec.md).
---

# Create Specification

Guide interactive requirements discovery and generate a comprehensive specification document (`spec.md`) for a feature or major change.

## Step 0. Discovery & Interview Phase

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it. use ask_user_question tool for asking user question.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Step 1 — Check working directory is clean
Run `git status` and check for uncommitted, unstaged, or
untracked files. If any exist, stop immediately and tell
the user to commit or stash changes before proceeding.
DO NOT CONTINUE until the working directory is clean.


## Step 2 — setup the git branch
Create a new git branch for the feature.
The branch name should follow this format: `feature/[ID]-[feature-name-kebab-case]`

Where:

1. `step_number` — zero-padded to 2 digits: 2 → 02, 11 → 11

2. `feature_title` — human readable title in Title Case
   - Example: "Registration" or "Login and Logout"

3. `feature_slug` — git and file safe slug
   - Lowercase, kebab-case
   - Only a-z, 0-9 and -
   - Maximum 40 characters
   - Example: registration, login-logout

4. `branch_name` — format: `feature/<feature_slug>`
   - Example: `feature/registration`

If you cannot infer these from user input, ask the user
to clarify before proceeding.


## Step 3 — Switch to main and pull latest
Run:
```
git checkout main
git pull origin main
```

## Step 4 — Create and switch to the feature branch
Run:
```
git checkout -b <branch_name>
```
## Step 5 — Research the codebase
Read these files before writing the spec:

- `AGENTS.md` — Global and project read both.

## Step 6. Storage Location

Write the completed specification to:
`.agents/specs/[ID]-[feature-slug]/spec.md`
