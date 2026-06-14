**Start a new project:**

bash

```bash
mkdir my-project
cd my-project
bun init
```

`bun init` asks a few questions (project name, entry point) and generates:

- `package.json`
- `tsconfig.json`
- `index.ts`

---

**Run a file:**

bash

```bash
bun run index.ts
```

**Add a package:**

bash

```bash
bun add hono
bun add -d @types/something  # -d for dev dependencies
```

---

That's the full setup. No separate TypeScript install, no `tsx`, no compiler step — Bun handles all of it. You're writing and running TypeScript in under a minute.


Same mental model as Go modules — you declare dependencies, they get locked, anyone can reproduce the exact same install.

**When you run `bun add hono`:**

1. Downloads the package
2. Adds it to `package.json` under `dependencies`
3. Creates/updates `bun.lockb` — the lockfile that pins exact versions

json

```json
// package.json after bun add hono
{
  "dependencies": {
    "hono": "^4.0.0"
  }
}
```

**`bun.lockb`** is binary (not human readable) — don't edit it, just commit it to git. It ensures everyone on the project gets identical versions.

---

**Key commands:**

bash

```bash
bun add hono          # add runtime dependency
bun add -d typescript # add dev-only dependency (not shipped)
bun remove hono       # remove
bun install           # install all dependencies from package.json (like go mod download)
```


**Global installs exist** but are rare — only for CLI tools you want available system-wide:

bash

```bash
bun add -g some-cli-tool
```

For project dependencies, always local.