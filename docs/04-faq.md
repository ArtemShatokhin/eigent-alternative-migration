# Eigent to Kortix migration FAQ: the open-source alternative

A team moving off Eigent asks the same questions in the same order. These are the answers, with the ones the earlier documents do not already cover. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector live in one git repo you own, and each session runs on its own cloud computer.

## Can I keep the agents and skills I built in Eigent?

Yes, as definitions. Eigent keeps an agent's instructions and a skill's steps inside a Space profile, and there is no documented export format, so you copy that text out by hand. Each agent becomes `agents/<name>.md`, each skill becomes a folder of markdown and scripts, and the project keeps both in the repo.

## Do I keep my model provider and API keys?

Yes. Kortix is model-agnostic, so a model provider you used in Eigent still works. You bring your own keys, or you can use the ChatGPT plan you already pay for. Key values never go into the repository: you set them with `kortix secrets set KEY=VALUE` and the agent reads them at run time.

## Where does the setup live after the move?

In files in a git repo you own. Agents and skills are markdown, company memory is files that accumulate, and `kortix.yaml` declares the machine image, the connectors, and the triggers. Eigent keeps the equivalent setup inside the desktop app, bound to your machine. In Kortix a second person can clone the repo, read the configuration, and diff a change before it lands.

## Can a job keep running after I close my laptop?

Yes, because the job is not on your laptop. Each session gets its own isolated sandbox on its own branch, and the agent can install, run, and break anything there. Only what it commits survives. Eigent's recommended setup runs agents locally on your desktop, so the work is tied to that machine while it runs.

## How does an agent's finished work get approved?

Through a change request. Session work reaches `main` through a change request a person reads as a diff, and merge is default-deny for agents, so nothing lands unreviewed. You list the open ones with `kortix cr ls` and merge the ones you accept. In Eigent you approve the result or ask for revisions inside the desktop app.

## How does the pricing compare?

Self-host is free on Kortix, and the team plan is a per-seat subscription. Eigent has a free tier when you bring your own keys or run a local model, then paid monthly plans billed with task credits. Prices and plan details change, so confirm each vendor on its own page: Eigent's [pricing page](https://www.eigent.ai/pricing) and the campaign site's [Eigent pricing breakdown](https://eigentalternative.com/eigent-pricing).

## Is Eigent open source too?

Yes. Eigent calls itself the open-source Cowork desktop application, built on the CAMEL framework, and its repository says the code can be downloaded, inspected, and modified ([Eigent on GitHub](https://github.com/eigent-ai/eigent)). Kortix is open source as well. What separates the two is where the work runs and who owns the configuration.

More questions and answers are on the campaign [FAQ](https://eigentalternative.com/faq). [Explore the open-source Eigent alternative](https://eigentalternative.com/) and start from one repo you own.
