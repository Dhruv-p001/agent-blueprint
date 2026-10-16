# Prompts

Three prompts from The Agent Blueprint. Each one works on its own: copy it, fill in anything in [square brackets], and paste it into your coding agent.

---

## 1. Plan-first prompt

Use this before any new feature or bigger change. It asks the agent for a plan and stops it from writing code until you approve.

```text
Before you write any code, I want a plan.

The task: [describe what you want built or changed]

Reply with:
1. Your understanding of the task, in 2 or 3 sentences.
2. The files you plan to create or change, and what each one is for.
3. Any new tools, functions or dependencies you want to add, and why each one is needed.
4. Anything you are unsure about. Ask me instead of guessing.
5. The simplest version that would work. If you think something more complex is needed, explain why.

Do not write or change any code yet. Wait until I reply with "approved" or with changes to the plan.
```

---

## 2. Tool review prompt

Use this when your project includes an AI agent with tools (functions the agent can call, like "look up a customer" or "send an email"). It finds overlap and unclear names.

```text
Review the tools in this project. A tool is any function the AI agent can call. Do not change anything yet.

For each tool, tell me:
- Its name and what it does, in one sentence.
- Whether another tool does the same or a very similar job.
- Whether its name and description make it clear when to use it.

Then suggest a shorter list of tools where:
- Each tool does one job.
- No two tools do the same job.
- Names are plain and specific.
- Each description says when to use the tool.

Show the current list and your suggested list side by side. Wait for my approval before making any changes.
```

---

## 3. Simplify-pass prompt

Use this after you finish each feature. It asks the agent to look for things to merge, remove or cut back, and to wait for your approval.

```text
We just finished a feature. Before we move on, do a simplify pass on the code we changed and the code around it. Do not change anything yet.

Look for:
1. Tools or functions that do the same or overlapping jobs and could be merged.
2. Logic that is repeated in more than one place.
3. Code, files or dependencies that nothing uses anymore.
4. Over-engineering: extra layers, settings or abstractions that the current feature does not need.

For each thing you find, tell me:
- Where it is (file and function).
- What you suggest.
- What could break, and how we would check.

Put the safest and most useful changes first. Then stop and wait. Only make the changes I approve, and run the tests after each one.
```
