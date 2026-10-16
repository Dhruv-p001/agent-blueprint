---
name: simplify-pass
description: Reviews the codebase or recent changes and proposes simplifications. Finds overlapping tools to merge, duplicated logic to remove, unused code to delete, and over-engineering to cut back. Lists findings and asks for approval before changing anything. Use after finishing a feature, or when the user asks to simplify, clean up or reduce complexity.
---

# Simplify pass

Review the code and propose ways to make it simpler. Change nothing until the user approves.

## 1. Decide the scope

- If the user names files, folders or a feature, review those.
- Otherwise, if the project uses git, review the files changed since the last commit plus the code they call or are called by. Use `git status` and `git diff` to find them.
- If there are no recent changes or no git history, review the whole codebase.
- Tell the user the scope in one line before you start.

## 2. Read the project rules

Read CLAUDE.md and AGENTS.md if they exist. Note how to run the tests and any rules about what must not change.

## 3. Look for four kinds of problems

1. **Overlapping tools or functions.** Two or more tools (functions an AI agent can call) or regular functions that do the same or nearly the same job. Example: `get_customer_email`, `get_customer_phone` and `get_customer_address` could be one `get_customer`.
2. **Duplicated logic.** The same steps written out in more than one place, even with small differences in names or formatting.
3. **Unused code.** Functions, files, settings or dependencies that nothing uses anymore.
4. **Over-engineering.** Layers, abstractions, settings or options that the current features don't need. Example: a plugin system with one plugin, or a config option nobody sets.

## 4. Check each finding before you report it

- Before calling something unused, search the whole project for its name, including tests, config files and scripts.
- Be careful with code that might be used from outside the project or by name at runtime (public APIs, command line entry points, anything loaded dynamically). Report it as "possibly unused" and say why you're unsure.
- Before suggesting a merge, check that the merged version would behave the same for every current caller.

## 5. Report

Give a numbered list, safest and most useful changes first. For each item include:

- **Where:** file and function or line
- **Problem:** what is overlapping, duplicated, unused or over-built
- **Suggestion:** what to change, in plain words
- **Risk:** what could break
- **Check:** how to confirm nothing broke (which tests to run, what to try by hand)

If you found nothing worth changing, say so. Don't invent changes to fill the list.

## 6. Ask for approval and stop

End with this question:

"Which of these should I apply? Reply with the numbers, 'all', or 'none'."

Then stop. Do not edit, create or delete any file until the user answers.

## 7. Apply only what was approved

- Make one approved change at a time.
- After each change, run the tests if the project has them. If a test fails, undo that change and tell the user.
- Don't change how the code behaves. A simplification should give the same results with less code.
- Don't edit tests just to make them pass.
- When you finish, list what you changed, what you skipped and why, and the test results.
