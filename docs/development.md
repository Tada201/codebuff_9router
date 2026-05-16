# Development

## Getting Started

Start the web server first:

```bash
bun up
```

Then start the CLI separately:

```bash
bun start-cli
```

### Building the CLI Binary

To build a standalone executable for the CLI:

```bash
cd cli
bun ./scripts/build-binary.ts codebuff-local 1.0.0
```

The binary will be generated in `cli/bin/codebuff-local.exe` (on Windows) or `cli/bin/codebuff-local` (on Unix).
 
 ### Local 9Router Testing
 
 For development and testing of the 9Router integration:
 1. Build the local binary as shown above.
 2. Run with the `--local` flag: `cli/bin/codebuff-local.exe --local`.
 3. Use slash commands `/9router endpoint` and `/9router key` to configure your local proxy.
 4. Verify LLM requests are correctly routed to your local proxy.

Other service commands:

```bash
bun ps    # check running services
bun down  # stop services
```

## Worktrees

To run multiple stacks on different ports, create `.env.development.local`:

```bash
PORT=3001
NEXT_PUBLIC_WEB_PORT=3001
NEXT_PUBLIC_CODEBUFF_APP_URL=http://localhost:3001
```

## Logs

Logs are in `debug/console/` (`db.log`, `studio.log`, `sdk.log`, `web.log`).

## Package Management

- Use `bun install`, `bun run ...` (avoid `npm`).

## Database Migrations

Edit schema using Drizzle's TS DSL (don't hand-write migration SQL), then run the internal DB scripts to generate/apply migrations.

## Running Scripts Against Prod

Scripts in `scripts/` connect to whatever environment Infisical injects. To run a script against the production database and services, prefix it with `infisical run --env=prod`:

```bash
infisical run --env=prod -- bun scripts/<name>.ts
```

You can also inline a one-off query:

```bash
infisical run --env=prod -- bun -e "import db from '@codebuff/internal/db'; /* ... */"
```

Add `--silent` to suppress the Infisical banner. Default env is `dev` — always pass `--env=prod` explicitly when you want prod. Prefer read-only queries; coordinate before running anything that writes.
