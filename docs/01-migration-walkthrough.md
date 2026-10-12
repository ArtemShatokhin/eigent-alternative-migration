# Migrating from Eigent to Kortix, the open-source AI Operating System

If your team has been running agents inside Eigent's desktop app and wants the same work to run for everyone, this is the order to move in. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and each session runs on its own cloud computer.

## What you are actually moving

Eigent keeps its setup inside a Space profile in the desktop app. The site describes a Space as the place where agents, skills, connectors, and instructions are packaged once and reused, with secrets kept on the machine ([Eigent](https://www.eigent.ai)). Your migration is the job of externalizing that profile into text files a repository can hold and a second person can read.

Four kinds of thing live in an Eigent Space, and each needs a home after the move:

- Agents, the personas that do the work.
- Skills, the reusable steps each agent follows.
- Connectors, the tools the agents reach.
- Settings, meaning the model provider, the keys, and the scheduled tasks.

The repository documents the local and self-hosted deployment modes, so the runtime side is well covered ([Eigent on GitHub](https://github.com/eigent-ai/eigent)). What the app does not document is an export format, so plan the move as a manual audit rather than a scripted dump.

## Step 1: Take inventory of the Eigent Space

Open each Space and write down everything it carries before you touch Kortix. The record you want:

- Every agent and the instructions behind it.
- Every skill and the steps it encodes.
- Every connector and which account it uses.
- The model provider you selected, and whether it is a cloud API, a gateway, or a local model.
- Every scheduled task, which Eigent shows alongside Sessions and Tasks, with its schedule.
- Any session output worth keeping for reference.

Treat this as the source-of-truth list. If a piece is missing from the inventory, it will be missing from the Kortix project.

## Step 2: Capture each definition as text

Because the Space lives in the app, you copy the definition text out by hand. For each agent, copy its purpose and instructions into a note. For each skill, copy the steps in order. For each connector, copy the provider name and the account it uses. For each scheduled task, copy the schedule in plain terms, such as "every weekday at 09:00".

Keep the keys out of these notes. In Eigent the secrets stay bound to your machine, and in Kortix secret values never enter the repository at all. Move the keys later through the secret store, not through a file you commit.

## Step 3: Build the Kortix project

Run `kortix init` in a new directory. It creates `kortix.yaml` plus the folders your agents and skills live in. Then place each piece from the inventory into the project:

| Eigent concept | Kortix | Where it goes |
| --- | --- | --- |
| An agent | `agents/<name>.md` for behavior, plus an `agents.<name>` block in `kortix.yaml` | `agents/` and `kortix.yaml` |
| A skill | A folder of markdown plus scripts | `skills/` |
| A connector | Declared in `kortix.yaml`, connected in the dashboard | `kortix.yaml` |
| A model provider and key | Your own key, set as a secret | Outside the repo |
| A scheduled task | A `cron` trigger | `kortix.yaml` |
| The Space profile itself | The project | The git repo |

Two details decide whether the rebuild holds. An agent is two files: the markdown file that carries its behavior and the block in `kortix.yaml` that carries its governance, meaning what it may reach. A trigger has exactly two types, `cron` and `webhook`, so a repeating Eigent automation becomes a cron trigger.

Connect the same tools and models you used before. Connectors reach 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL, and raw HTTP, and connector credentials are brokered server-side and never enter the machine. Any model provider works with your own keys, or you can bring the ChatGPT plan you already pay for.

## Step 4: Ship the project

Run `kortix ship`. It pushes the repository and brings the whole thing live. At this point the configuration that used to sit in one desktop app is a repo every member of the team can clone and read.

## Step 5: Run a session instead of running the app

Start the work with a session:

```
kortix sessions new --prompt "Summarize this week's tickets and open a change request with the digest"
```

Each session gets its own isolated sandbox on its own branch. The agent can install, run, and break anything it wants, and only what it commits survives. This is the change a desktop user notices first: the job no longer runs on the machine in front of you, so it keeps going after you close the laptop.

## Step 6: Review the work as a change request

Where Eigent asks you to approve a result inside the app, Kortix takes the work back to `main` through a change request. List the ones agents have opened:

```
kortix cr ls
```

Merge is default-deny for agents, so a person reads the diff before it lands. That is the part that changes the review, because a change request is a diff on a branch that a second person can read, not an approval screen on one desktop.

## What does not carry over

- A desktop-local file path becomes a path inside a session's sandbox, not on your laptop.
- An in-app approval becomes a reviewed change request on the branch.
- A Space bound to one machine becomes one repo the whole team can clone.

Plan for those three differences, and the rest of the move is mechanical.

## Where to go next

The [Kortix vs Eigent comparison](02-kortix-vs-eigent.md) covers the decision in detail. On the campaign site, [the open-source Eigent alternatives](https://eigentalternative.com/eigent-alternatives) places Kortix next to the rest of the field, and [Kortix vs Eigent](https://eigentalternative.com/kortix-vs-eigent) is the buyer's view.

Start the move from one repo you own. [Explore the open-source Eigent alternative](https://eigentalternative.com/) and keep the configuration in your own repo.
