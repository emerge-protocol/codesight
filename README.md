# codesight

Your AI assistant wastes thousands of tokens every conversation just figuring out your project structure. codesight fixes that in one command.

```bash
npx codesight
```

It scans your codebase and generates a compact, structured context map that any AI coding tool can read instantly. Routes, database schema, components, dependencies, environment variables, middleware, all of it, mapped and ready.

Your AI starts every conversation already understanding your project. No more watching it grep through files, read configs, and burn tokens on exploration.

Works with **Claude Code, Cursor, GitHub Copilot, OpenAI Codex, Windsurf, Cline**, and anything that reads markdown.

## How It Works

Run `npx codesight` in any project. It produces a `.codesight/` directory:

```
.codesight/
  CODESIGHT.md     Combined context map (one file, full project understanding)
  routes.md        Every API route with method, path, params, and what it touches
  schema.md        Every database model with fields, types, keys, and relations
  components.md    Every UI component with its props
  libs.md          Every library export with function signatures
  config.md        Every env var (flagged as required or has default), config files, key deps
  middleware.md    Auth, rate limiting, CORS, validation, logging, error handlers
  graph.md         Which files import what, and which files break the most things if changed
  report.html      Visual dashboard you can open in a browser
```

Point your AI assistant at `CODESIGHT.md` and it has complete project context in a single read.

## What You Get

**Your routes, fully mapped.** Not just paths. Methods, URL parameters, what each route touches (auth, database, cache, payments, AI, email, queues), and where the handler lives.

```markdown
- `POST` `/auth/login` [auth, db, email]
- `GET` `/api/projects/:id/analytics` params(id) [auth, db, cache]
- `POST` `/api/billing/checkout` [auth, db, payment]
- `POST` `/api/webhooks/stripe` [payment]
```

**Your database schema, instantly readable.** Models, fields, types, primary keys, foreign keys, unique constraints, relations. No need to open migration files or ORM configs.

```markdown
### users
- id: uuid (pk, default)
- email: text (unique, required)
- plan: text (required, default)
- stripeCustomerId: text (fk)

### projects
- id: uuid (pk, default)
- userId: uuid (fk)
- name: text (required)
- domain: text (unique)
- _relations_: userId -> users.id
```

**Your dependency graph, with blast radius.** The files that are imported the most are the ones that break the most things when changed. codesight surfaces them so your AI knows to be careful.

```markdown
## Most Imported Files (change these carefully)
- `packages/shared/src/index.ts` — imported by **14** files
- `apps/api/src/lib/db.ts` — imported by **9** files
- `apps/api/src/lib/auth.ts` — imported by **7** files
```

**Your env vars, audited.** Which ones are required, which ones have defaults, and where each one is referenced. No more hunting through `.env.example` files.

```markdown
- `DATABASE_URL` **required** — apps/api/src/lib/db.ts
- `JWT_SECRET` **required** — apps/api/src/lib/auth.ts
- `PORT` (has default) — apps/api/src/index.ts
```

**Token savings, measured.** codesight shows you exactly how much context it provides versus how much your AI would spend exploring the same information manually.

```
Output size:      ~3,200 tokens
Exploration cost: ~52,000 tokens (without codesight)
Saved:            ~48,800 tokens per conversation
```

## Works With Every Stack

codesight is not tied to one framework or one language.

**Routes:** Hono, Express, Fastify, Next.js (App Router + Pages), Koa, FastAPI, Flask, Django, Go (net/http, Gin, Fiber)

**Schema:** Drizzle, Prisma, TypeORM, SQLAlchemy

**Components:** React, Vue, Svelte. Automatically filters out shadcn/ui and Radix primitives so you only see your own components.

**Libraries:** TypeScript, JavaScript, Python, Go. Extracts exported functions with signatures, classes, interfaces, types, enums.

**Monorepos:** pnpm workspaces, npm workspaces, yarn workspaces. Detects frameworks and ORMs across all workspace packages automatically.

## One Command AI Setup

```bash
npx codesight --init
```

Generates ready-to-use instruction files for every major AI coding tool at once:

- **CLAUDE.md** for Claude Code
- **.cursorrules** for Cursor
- **.github/copilot-instructions.md** for GitHub Copilot
- **codex.md** for OpenAI Codex CLI
- **AGENTS.md** for OpenAI Codex agents

Each file is pre-filled with your project's stack, architecture overview, high-impact files, and required environment variables. Your AI assistant reads it automatically and starts with full context from the first message.

## Visual Report

```bash
npx codesight --open
```

Opens an interactive HTML dashboard in your browser showing your full project map: routes table, schema cards, dependency impact bars, env var audit, middleware overview, and a token savings breakdown. Useful for onboarding new team members or just seeing your project from above.

## MCP Server

```bash
npx codesight --mcp
```

Runs codesight as a Model Context Protocol server. Claude Code and Cursor can call it directly to get project context on demand.

Add to your Claude Code or Cursor MCP config:

```json
{
  "mcpServers": {
    "codesight": {
      "command": "npx",
      "args": ["codesight", "--mcp"]
    }
  }
}
```

## Stays Fresh Automatically

**Watch mode** re-scans when files change:

```bash
npx codesight --watch
```

**Git hook** regenerates context on every commit:

```bash
npx codesight --hook
```

## All Options

```bash
npx codesight                       # Scan current directory
npx codesight ./my-project          # Scan specific directory
npx codesight --init                # Generate AI config files
npx codesight --open                # Open visual HTML report
npx codesight --html                # Generate HTML report without opening
npx codesight --mcp                 # Start MCP server
npx codesight --watch               # Watch mode
npx codesight --hook                # Install git pre-commit hook
npx codesight --json                # Output as JSON
npx codesight -o .ai-context        # Custom output directory
npx codesight -d 5                  # Limit directory depth
```

## Zero Dependencies

codesight has zero runtime dependencies. Only Node.js built-ins. Installs in seconds, runs anywhere Node runs, no supply chain risk.

## License

MIT
