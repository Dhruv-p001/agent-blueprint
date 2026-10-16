# Responsibility map

Fill this in before you build. Each row is one job your project needs done. Decide who owns it and write down why.

**Owned by** can be:

- **System:** your database, files or regular code. Anything that stores data or keeps track of state (where things currently stand, like which step a task is on).
- **Agent:** the AI. Jobs that need reading, writing or judgement with language.
- **Person:** a human. Decisions with real consequences, like money, deleting data, or anything sent to customers.

A simple rule: the system keeps track of data and state, and the agent asks for what it needs.

## Example

An agent that answers customer questions about orders.

| Responsibility | Owned by (system, agent, or person) | Why |
| --- | --- | --- |
| Storing the status of each order | System | The database is the single source of truth. The agent looks the status up and never guesses or keeps its own copy. |
| Writing the reply to the customer | Agent | Turning order details into a clear, friendly answer is language work. |
| Approving refunds above a set amount | Person | Money decisions need a human sign-off. |

## Your project

Project: [name]

| Responsibility | Owned by (system, agent, or person) | Why |
| --- | --- | --- |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
