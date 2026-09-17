---
name: create-tasks
description: use for generating tasks (tasks.md).
---

# creating tasks for subagents to implement

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

## step 5. Principle to follow when creating tasks
I need you to organize the work in phases and make tasks in tasks.md so we can hand off the phased work to different subagents in chunks that can either be done sequentially and/or parallelized. Be sure the todos have a [Phase X] refer it in its content


## step 6. write the prompt for every task.

- use 'writing-for-subagents' skill invoke it and follow the instruction inside of it and use this to create the prompt for every subagent.

- each prompt should include the relavent skill name and the path to that skill and explicit instruction so the subagent must invoke and use that skill.

- subagent prompt include the Relavent Search MCP tool that the subagent can use to fetch live internet data/docs to get unstuck. 

- The subagent should have explicit instruction of verification. The subagent should implement what its told and don't run any test, typechecks, lints that the prompt don't specify.


## Step 7. Storage Location

Write the completed tasks to:
`.agents/specs/[ID]-[feature-slug]/tasks.md`

## step 9: ask the user for permission.

ask the user with ask_user_question tool to implement the tasks.md right away. 


