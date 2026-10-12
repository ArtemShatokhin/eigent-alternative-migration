# Kortix is the open-source AI Operating System

A migration guide for teams that have been running Eigent, a desktop multi-agent Cowork app, and want the same work on a platform the whole company owns. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and each session runs on its own cloud computer.

Eigent is genuinely good at what it does. It runs a multi-agent workforce locally on your own machine, keeps your keys and models on that machine, and lets you inspect and approve each result. Its own site describes a Space where agents, skills, connectors, and instructions are packaged once and reused, with secrets kept on the machine ([Eigent](https://www.eigent.ai)). Teams outgrow that model at the point where the work has to be shared, scheduled, and reviewed by more than one person, and when the configuration needs to live somewhere other than one laptop.

## Who this is for

- A team already using Eigent Spaces, agents, skills, and connectors that wants the same work to run for everyone, not one desktop at a time.
- An engineer who has to rebuild that setup as a repository and wire it to infrastructure the company controls.
- A team evaluating the move that needs the comparison, the steps, and the self-hosting path in one place.

## The migration in one pass

1. Take inventory of what your Eigent Space holds: its agents, skills, connectors, instructions, model providers, and scheduled tasks.
2. Write each definition into a Kortix project as text: `agents/`, `skills/`, `memory/`, and `kortix.yaml`.
3. Connect the same tools and models to the project, with secret values stored outside the repo.
4. Run `kortix ship` and start a session. A session replaces the desktop run and gets its own cloud computer and branch.
5. Review finished work as a change request instead of an in-app approval, then merge it.

The step-by-step version is in the [migration walkthrough](docs/01-migration-walkthrough.md).

## Documents

- [Migrating from Eigent to Kortix](docs/01-migration-walkthrough.md): what to move out of Eigent, how to model each piece in a repo, and how sessions and change requests replace the desktop app.
- [Kortix vs Eigent](docs/02-kortix-vs-eigent.md): the comparison built from Eigent's own repository and site, and where a team outgrows a desktop app.
- [Self-hosting Kortix](docs/03-self-hosting.md): the verified commands for running the open-source stack on your own Linux box.
- [FAQ](docs/04-faq.md): the questions teams ask before they move.

## Where to read more

- [The open-source Eigent alternative](https://eigentalternative.com/) on the campaign site.
- [Kortix vs Eigent](https://eigentalternative.com/kortix-vs-eigent) for the buyer's comparison.
- [Self-host an open-source Eigent alternative](https://eigentalternative.com/self-hosting-eigent-alternative) for the hosted walkthrough.

Kortix is open source. Read it, fork it, audit it. The code is at [Kortix on GitHub](https://github.com/kortix-ai/suna).
