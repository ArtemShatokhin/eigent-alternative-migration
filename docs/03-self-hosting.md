# Self-hosting Kortix, an open-source Eigent alternative, on your own box

Kortix is open source and self-host is free, so the whole platform runs on a Linux box you control. Eigent runs a Cowork desktop app on your machine ([Eigent](https://www.eigent.ai)); the Kortix stack runs as one Docker Compose set, while each agent session runs on a separate sandbox provider. This is the verified path from a bare box to a running instance.

## What the stack is

One Docker Compose stack holds the frontend, the API, the LLM gateway, and the Supabase distribution. Agent sessions do not run on that stack. They run on a separate sandbox provider, with Daytona as the default and Platinum and E2B also supported. Keep that split in mind, because self-hosting the platform and running a session are two different machines.

## The one-shot bootstrap

On a bare Linux box, the repository ships a one-shot bootstrap script, `scripts/kortix-selfhost-up.sh`, that installs Docker, installs the `kortix` CLI, and starts the stack in a single run. Fetch it from the [Kortix on GitHub](https://github.com/kortix-ai/suna) repository on the `dev` branch, then run it with your domain and an email for the TLS certificate:

```
bash scripts/kortix-selfhost-up.sh --domain kortix.example.com --email ops@example.com
```

The script runs on Linux only. On another operating system, install the CLI and use the manual path instead.

## The manual path

1. Install the CLI.

   ```
   curl -fsSL https://kortix.com/install | bash
   ```

2. Point DNS at the box. Create an A or AAAA record for your domain and for `api.<domain>`, both pointing at the box's IP address, then open ports 80 and 443. The bundled Caddy proxy uses those ports to issue a TLS certificate.

3. Initialize with your domain.

   ```
   kortix self-host init --domain kortix.example.com
   ```

4. Start the stack.

   ```
   kortix self-host start
   ```

5. Watch it come up, then set your sandbox provider key.

   ```
   kortix self-host status
   kortix self-host configure
   ```

   `configure` is an interactive prompt for the sandbox provider key and, optionally, a managed-git token. Self-hosted instances use your own model key by default, so connect it in the model picker after you sign up in the dashboard.

## Evaluation mode with no domain

To try Kortix without a domain, use a Cloudflare tunnel:

```
kortix self-host init --tunnel cloudflare
kortix self-host start
```

The tunnel URL changes on every restart, so use this mode to evaluate the stack, not to run it in production.

## Tuning, updates, and backups

- Memory limit. Each API container has a 640 MiB memory limit by default. Keep the default on an 8 GiB host. On a 16 GiB host, raise it when API traffic reaches the limit:

  ```
  kortix self-host env set KORTIX_API_MEMORY_LIMIT=1024m
  ```

  Confirm the applied limit with `docker stats --no-stream`.

- Updates. Every instance updates itself automatically. To pin an exact version, run `kortix self-host update --tag 0.9.84`. To turn the updater off, pass `--auto-update off`.

- Backups. Kortix has no separate backup system. Each instance stores its data as two directories under `~/.config/kortix/self-host/<instance>/`: `volumes/db/data` for the Postgres database and `volumes/storage` for file storage. The instance's `.env` file holds every secret and signing key it uses. Back up all three before you run a destructive command.

## What it needs to run

The stack needs the internet. The bootstrap and updates pull images from docker.io, and the instance must be reachable so a sandbox can call back to it. Self-host it on Kortix Cloud, in your VPC, or on your own on-prem network; the free path is the same stack either way.

The full command surface is in the Kortix self-hosting docs. [Read the docs](https://kortix.com/docs/host). The code is at [Kortix on GitHub](https://github.com/kortix-ai/suna).

The hosted version of this walkthrough is at [self-hosting an open-source Eigent alternative](https://eigentalternative.com/self-hosting-eigent-alternative). [Explore the open-source Eigent alternative](https://eigentalternative.com/) and run the whole platform on a box you control.
