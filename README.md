# Stop your AI agent from overbuilding

AI coding agents are quick to add code and slow to remove it, so a simple project can turn into a tangled one without anyone deciding it should.

**[Download the PDF guide (8 pages)](agent-blueprint.pdf)**

This is The Agent Blueprint: 5 steps to keep what your agent builds small and clean, plus a starter kit of templates and prompts you can copy into your own projects. It works with any AI coding agent, like Claude Code, Cursor or Codex. You don't need to be an expert to use it.

---

## Why agents overbuild

AI coding agents are very good at following instructions. Ask for a feature and you'll get one, usually fast.

The catch is that agents tend to solve problems by adding something: a new file, a new function, a new helper. They rarely stop on their own to ask whether the structure still makes sense, or whether something could be removed instead.

One addition is fine. Weeks of them, and the project gets harder to read and harder to change, for you and for the agent.

Warning signs to watch for:

- **Too many tools.** Several tools do almost the same thing, and the agent keeps picking the wrong one.
- **The same logic in several places.** You fix a bug in one spot and it's still there in another.
- **Files nobody asked for.** New helpers, wrappers and config files show up that the task didn't need.
- **Every fix adds more code.** Problems get solved by stacking code on top, never by removing or rethinking.

The agent is doing what it was asked. So the fix is on our side: ask for different things, at the right moments. That's what the 5 steps are for.

---

## The 5 steps

### Step 1. Plan before it builds

Before the agent writes anything, ask it what it plans to do. Read the plan, push back on anything bigger than the task, then approve it.

Reading a plan takes a few minutes. Untangling code you never wanted takes a lot longer.

When you read the plan, look out for new files, new dependencies (outside libraries or packages the code will rely on) and anything that sounds like "in case we need it later".

**Using Claude Code?** It has a plan mode. In plan mode, Claude looks through your code and proposes a plan without editing any files, and waits for you to approve it. In any other agent, use the prompt below.

**Plan-first prompt** (copy and paste, then fill in the task):

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

### Step 2. Decide who does what

When you build an AI agent, some jobs belong to the agent and some belong to the system around it. The system means your database, your files and your regular code.

A simple rule: **the system keeps track of data and state, and the agent asks for what it needs.** State means where things currently stand, like which step an order is on or which tasks are done.

When an agent has to remember things itself, it starts building its own copies: extra variables, notes, little tracking files. Those copies drift out of date, and then you have two versions of the truth.

Some jobs belong to a person. Anything with real consequences, like money, deleting data or messages to customers, should get a human sign-off until you trust the setup.

Fill in a responsibility map before you build. Each row is one job. Decide who owns it and write down why.

Example: an agent that answers customer questions about orders.

| Responsibility | Owned by (system, agent, or person) | Why |
| --- | --- | --- |
| Storing the status of each order | System | The database is the single source of truth. The agent looks the status up and never guesses or keeps its own copy. |
| Writing the reply to the customer | Agent | Turning order details into a clear, friendly answer is language work. |
| Approving refunds above a set amount | Person | Money decisions need a human sign-off. |
|  |  |  |
|  |  |  |

A blank version is in [starter-kit/responsibility-map.md](starter-kit/responsibility-map.md).

### Step 3. Fewer, clearer tools

A tool is a function an AI agent can call to do something: look up a customer, send an email, create a ticket. Each time the agent acts, it picks one of its tools.

More tools means more choices, and more chances to pick the wrong one. Coding agents also tend to add a new tool for every small need, so the list grows quietly.

Four rules:

1. **One job per tool.**
2. **No two tools doing the same job.**
3. **Clear names.** `get_customer` tells you what it does. `fetch_data_v2` doesn't.
4. **Descriptions that say when to use the tool.** The description is what the agent reads when it decides which tool to pick.

**Before:** three tools that each fetch one detail.

```text
get_customer_email(customer_id)
get_customer_phone(customer_id)
get_customer_address(customer_id)
```

**After:** one tool.

```text
get_customer(customer_id)
  Description: Look up one customer by their ID. Use this whenever you
  need any customer details. Returns name, email, phone and address.
```

The agent has fewer choices, and there's one place to fix if customer data changes.

**Tool review prompt** (finds overlap in an existing project):

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

### Step 4. Write the rules down

By default, an agent doesn't carry your earlier conversations into a new session. Whatever you explained yesterday, you'd have to explain again.

A project rules file fixes this. It's a plain text file in your project that the agent reads at the start of each session. Write your rules once and they apply every time.

- **AGENTS.md** is a common convention that several coding agents read.
- **CLAUDE.md** is the file Claude Code reads.

The content can be the same. Only the file name changes. Both use Markdown, a simple text format where `#` makes a heading and `-` makes a bullet point.

Keep it short and specific. Long, vague rules are easy to miss.

A short example:

```markdown
# AGENTS.md

## About this project
- An app that tracks customer orders. Python with FastAPI.
- Run the tests with: pytest

## How to work
- Plan first. Wait for my approval before writing code.
- Ask when unsure instead of guessing.
- Pick the simplest solution that works.

## Code rules
- Reuse existing functions before writing new ones.
- Don't add new dependencies without asking.
- When you replace code, delete the old version.
```

Full templates for both files are in [starter-kit/AGENTS.md](starter-kit/AGENTS.md) and [starter-kit/CLAUDE.md](starter-kit/CLAUDE.md).

### Step 5. Ask it to clean up

Agents are good at adding things and rarely suggest removing them. In practice, an agent won't go back and simplify code that already works unless you ask.

So ask. After each feature, run a simplify pass: the agent reviews what was built and suggests what to merge, remove or cut back. You approve the changes you want.

Doing it after each feature keeps each pass small. A cleanup after months of work is a much bigger job.

**Simplify-pass prompt:**

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

What to check in the agent's answer:

- Tools that overlap and could be merged
- Logic repeated in more than one place
- Code, files or dependencies nothing uses
- Layers or settings the feature doesn't need
- Tests still pass after each change

**Using Claude Code?** The starter kit includes a simplify-pass skill that does this review step by step. See below.

---

## How to use the starter kit

Everything is in the [starter-kit](starter-kit) folder. "Project root" below means the top folder of your project, the one that holds your README.

| File | What it is | Where it goes |
| --- | --- | --- |
| `AGENTS.md` | Project rules file for most coding agents | Copy to your project root |
| `CLAUDE.md` | The same rules, set up for Claude Code | Copy to your project root if you use Claude Code |
| `prompts.md` | The plan-first, tool review and simplify-pass prompts | Keep it somewhere handy and copy prompts when you need them |
| `responsibility-map.md` | Fill-in table for Step 2 | Copy into your project (for example a `docs` folder) and fill it in |
| `.claude/skills/simplify-pass/SKILL.md` | A Claude Code skill for Step 5 | See below |

**Setting up the rules files**

1. Copy `AGENTS.md` (and `CLAUDE.md` if you use Claude Code) into your project root.
2. Open it and replace everything in [square brackets] with your own details.
3. Delete any rule that doesn't fit your project. Short is better.

**Adding the simplify-pass skill to a project**

A Claude Code skill is a folder with a `SKILL.md` file inside. The file holds instructions Claude can follow when they fit the task.

1. Copy the `.claude` folder from the starter kit into your project root. You should end up with `your-project/.claude/skills/simplify-pass/SKILL.md`.
2. If your project already has a `.claude` folder, copy only the `skills/simplify-pass` folder into it.
3. After you finish a feature, ask Claude: "Do a simplify pass." It will list what it suggests and wait for your approval before changing anything.

Can't see the `.claude` folder? Folders whose names start with a dot are hidden by default on a Mac. In Finder, press Cmd + Shift + . (period) to show them.

---

## Checklist

- [ ] **Plan before it builds.** I asked for a plan and approved it before any code was written.
- [ ] **Decide who does what.** The system keeps track of data and state, and every job has an owner.
- [ ] **Fewer, clearer tools.** Each tool does one job, none overlap, and each description says when to use it.
- [ ] **Write the rules down.** The project has an AGENTS.md or CLAUDE.md with how to run tests and the plan-first rule.
- [ ] **Ask it to clean up.** I ran a simplify pass after this feature, and the tests still pass.

---

Made by Dhruv Patel · [solwise.ai](https://solwise.ai)

Tried it on a project? I'd like to hear what worked and what didn't. Open an issue here or get in touch through solwise.ai.

Released under the [MIT License](LICENSE).
