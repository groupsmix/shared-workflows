# shared-workflows

Reusable GitHub Actions workflows for **Node.js**, **Python**, **Next.js**, and mixed projects.

Use these workflows from any repo by referencing them with `workflow_call`.

---

## Workflows

| Workflow | File | Description |
|----------|------|-------------|
| **CI** | `.github/workflows/ci.yml` | Install dependencies, lint, and test |
| **Build** | `.github/workflows/build.yml` | Build the project and optionally upload artifacts |
| **Deploy** | `.github/workflows/deploy.yml` | Deploy on merge to main (or any trigger) |

---

## Inputs Reference

### CI (`ci.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `language` | Yes | `node` | `node`, `python`, or `both` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `lint-command` | No | — | Custom lint command |
| `test-command` | No | — | Custom test command |
| `install-command` | No | — | Custom install command |
| `working-directory` | No | `.` | Working directory for commands |

### Build (`build.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `language` | Yes | `node` | `node`, `python`, or `both` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `build-command` | No | — | Custom build command |
| `install-command` | No | — | Custom install command |
| `working-directory` | No | `.` | Working directory for commands |
| `upload-artifact` | No | `false` | Upload build output as artifact |
| `artifact-name` | No | `build-output` | Name of the artifact |
| `artifact-path` | No | `dist` | Path to upload |

### Deploy (`deploy.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `deploy-command` | **Yes** | — | The deploy command to run |
| `environment` | No | `production` | GitHub environment name |
| `language` | No | `node` | `node`, `python`, or `other` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `install-command` | No | — | Custom install command |
| `build-command` | No | — | Build command to run before deploy |
| `working-directory` | No | `.` | Working directory for commands |

**Secrets:**

| Secret | Required | Description |
|--------|----------|-------------|
| `deploy-token` | No | Deployment token (Vercel, Netlify, AWS, etc.) |

---

## Usage Examples

### Node.js project (npm)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: node
      node-version: "20"
      package-manager: npm
```

### Next.js project (pnpm)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: node
      node-version: "20"
      package-manager: pnpm

  build:
    uses: groupsmix/shared-workflows/.github/workflows/build.yml@main
    with:
      language: node
      node-version: "20"
      package-manager: pnpm
      upload-artifact: true
      artifact-path: .next
```

### Python project (pip)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: python
      python-version: "3.12"
      package-manager: pip
```

### Python project (Poetry)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: python
      python-version: "3.12"
      package-manager: poetry
```

### Mixed project (Node.js + Python)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: both
      node-version: "20"
      python-version: "3.12"
      package-manager: npm
```

### Custom commands

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: node
      node-version: "18"
      package-manager: yarn
      lint-command: "yarn eslint src/ --ext .ts,.tsx"
      test-command: "yarn jest --coverage"
```

### Build + Deploy on merge to main

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  build:
    uses: groupsmix/shared-workflows/.github/workflows/build.yml@main
    with:
      language: node
      package-manager: npm
      upload-artifact: true

  deploy:
    needs: build
    uses: groupsmix/shared-workflows/.github/workflows/deploy.yml@main
    with:
      language: node
      package-manager: npm
      build-command: "npm run build"
      deploy-command: "npx vercel --prod --token $DEPLOY_TOKEN"
    secrets:
      deploy-token: ${{ secrets.VERCEL_TOKEN }}
```

### Deploy a Python app (e.g., to Railway)

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: groupsmix/shared-workflows/.github/workflows/deploy.yml@main
    with:
      language: python
      package-manager: poetry
      deploy-command: "railway up --service my-app"
    secrets:
      deploy-token: ${{ secrets.RAILWAY_TOKEN }}
```

---

## How It Works

These workflows use [`workflow_call`](https://docs.github.com/en/actions/using-workflows/reusing-workflows) to act as reusable templates. Your repo calls them like a function, passing in the inputs relevant to your project.

**Supported ecosystems:**

- **Node.js** — npm, yarn, pnpm
- **Python** — pip, poetry
- **Frameworks** — Next.js, React, FastAPI, Django, Flask, and anything that uses standard build/test/lint commands

**Key features:**

- Dependency caching for fast CI runs
- Configurable lint, test, build, and deploy commands
- Artifact upload support for build outputs
- GitHub Environments support for deploy approvals
- Secret passing for deploy tokens

---

## License

MIT
