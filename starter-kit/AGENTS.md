# AGENTS.md

Project rules for AI coding agents. Read this file at the start of every session and follow it.

<!--
How to use this template:
1. Copy this file into the top folder of your project (the same folder as your README).
2. Replace everything in [square brackets] with details about your project.
3. Delete any rule that doesn't fit. Keep the file short.
-->

## About this project

- What it does: [one or two sentences]
- Main language and framework: [for example, Python with FastAPI]
- How to run it: [command]
- How to run the tests: [command]

## How to work

1. Plan first. For any change bigger than a small fix, write a short plan and wait for my approval before writing code.
2. Ask when unsure. If something is unclear, ask a question instead of guessing.
3. Keep changes small. Only change what the task needs. Ask before refactoring (restructuring) code the task doesn't touch.
4. Pick the simplest solution that works. Explain why before adding a new layer, abstraction or pattern.

## Code rules

- Before writing a new function, check whether one already exists that does the job. Reuse it.
- Don't repeat logic. If the same code is needed in two places, move it into one shared function.
- Don't add new dependencies (outside libraries or packages) without asking.
- Don't create new files unless the task needs them.
- When you replace code, delete the old version. Don't leave it behind "just in case".

## Tools (if this project includes an AI agent)

- One job per tool.
- No two tools that do the same job.
- Clear, specific names.
- Each tool description says when to use the tool.

## Who owns what

- The system (database, files, regular code) keeps track of data and state.
- The agent asks the system for what it needs. It does not keep its own copy.
- [Optional: link to your responsibility map, for example docs/responsibility-map.md]

## After each feature

Do a simplify pass. Look for overlapping tools, repeated logic, unused code and over-engineering. List what you find, suggest changes, and wait for my approval before changing anything.

## Never

- [for example: Never commit passwords, API keys or other secrets]
- [for example: Never edit generated files by hand]
