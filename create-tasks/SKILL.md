---
name: create-tasks
description: use for generating tasks (tasks.md).
---

## step 0: confirm the spec/design.

if the user does not provide the reference to spec.md or design.md then try to read the last created spec from the root '.agents/specs/[ID]-[feature-slug]' in the each spec there will be spec.md or design.md and can be both. if no spec exists then use the ask_user_question tool to ask the user to create the spec first with 'create-spec' skill.

## step 1: organize the tasks for subagents.
after chocing spec to work with then organize the work in phases and make tasks from design.md/spec.md so we can hand off the phased work to different sub_agents with workflows in chunks that can either be done sequentially and/or parallelized. Be sure the todos have a [Phase X] refer it in its content


## step 3: write the prompt for every phase and task.

now after organizing the spec into tasks now use 'writing-for-subagents' skill invoke it and follow the instruction inside of it and use this to create the prompt for each task and the prompt should follow the best practices inside of writing-for-subagents each prompt should include the relavent skill name and the path to that skill so the subagent can invoke that skill and follow the guidance of the skill and also reference the mcp tool the web search and the web fetch when the subagent get stuck then the subagent can use the web fetch and web searhc to get unstuck. 

## step 4: save the file tasks.md

create tasks.md file and save all the instruction. save it in the same folder where the spec existed.

## step 5: ask the user for permission.

ask the user with ask_user_question tool to implement the tasks.md right away. 


