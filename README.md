# shared-workflows

Reusable GitHub Actions workflows for **Node.js**, **Python**, **Next.js**, and mixed projects.

Use these workflows from any repo by referencing them with `workflow_call`.

---

## Workflows

| Workflow | File | Description |
|----------|------|-------------|
| **CI** | `ci.yml` | Install dependencies, lint, and test |
| **Build** | `build.yml` | Build the project and optionally upload artifacts |
| **Deploy** | `deploy.yml` | Deploy on merge to main (or any trigger) |
| **Security** | `security.yml` | Dependency vulnerability scanning + CodeQL SAST |
| **Release** | `release.yml` | Automated versioning, changelog, and GitHub releases |
| **Docker** | `docker.yml` | Build and push Docker images (GHCR, DockerHub, ECR) |
| **Preview** | `preview.yml` | Deploy PR preview environments (Vercel, Netlify, custom) |
| **Code Quality** | `code-quality.yml` | Formatters, type checking, and coverage reporting |
| **Notify** | `notify.yml` | Slack and Discord notifications |
| **Stale** | `stale.yml` | Auto-close stale issues and PRs |
| **Auto Label** | `label.yml` | Auto-label PRs based on changed file paths |

All workflows are in `.github/workflows/`.

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

**Secrets:** `deploy-token` — Deployment token (Vercel, Netlify, AWS, etc.)

### Security (`security.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `language` | Yes | `node` | `node`, `python`, or `both` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `run-codeql` | No | `false` | Enable CodeQL static analysis |
| `codeql-languages` | No | `javascript` | Comma-separated CodeQL languages |
| `severity-threshold` | No | `high` | Minimum severity to fail on: `low`, `moderate`, `high`, `critical` |
| `working-directory` | No | `.` | Working directory for commands |

### Release (`release.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `language` | No | `node` | `node`, `python`, or `other` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `release-type` | No | `semantic-release` | `semantic-release`, `conventional-changelog`, or `manual` |
| `draft` | No | `false` | Create release as a draft |
| `prerelease` | No | `false` | Mark release as a prerelease |
| `generate-changelog` | No | `true` | Auto-generate release notes |
| `tag-prefix` | No | `v` | Tag prefix |
| `working-directory` | No | `.` | Working directory for commands |

**Secrets:** `npm-token` — NPM publish token | `pypi-token` — PyPI publish token

### Docker (`docker.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `image-name` | **Yes** | — | Docker image name |
| `registry` | No | `ghcr` | `ghcr`, `dockerhub`, or `ecr` |
| `dockerfile` | No | `Dockerfile` | Path to Dockerfile |
| `context` | No | `.` | Docker build context path |
| `platforms` | No | `linux/amd64` | Target platforms (comma-separated) |
| `build-args` | No | — | Build args (newline-separated `KEY=VALUE`) |
| `push` | No | `true` | Push image after build |
| `tag-strategy` | No | `semver` | `semver`, `sha`, `branch`, or `custom` |
| `custom-tags` | No | — | Custom tags (when strategy is `custom`) |
| `cache` | No | `true` | Enable Docker layer caching |

**Secrets:** `registry-username` — Registry username | `registry-password` — Registry password/token

### Preview (`preview.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `provider` | No | `vercel` | `vercel`, `netlify`, or `custom` |
| `language` | No | `node` | `node`, `python`, or `other` |
| `node-version` | No | `20` | Node.js version |
| `package-manager` | No | `npm` | `npm`, `yarn`, or `pnpm` |
| `build-command` | No | — | Custom build command |
| `install-command` | No | — | Custom install command |
| `deploy-command` | No | — | Custom deploy command (for custom provider) |
| `working-directory` | No | `.` | Working directory for commands |
| `vercel-org-id` | No | — | Vercel organization ID |
| `vercel-project-id` | No | — | Vercel project ID |

**Secrets:** `deploy-token` — Deploy token (Vercel, Netlify, etc.)

### Code Quality (`code-quality.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `language` | Yes | `node` | `node`, `python`, or `both` |
| `node-version` | No | `20` | Node.js version |
| `python-version` | No | `3.12` | Python version |
| `package-manager` | No | `npm` | `npm`, `yarn`, `pnpm`, `pip`, or `poetry` |
| `run-formatter` | No | `true` | Run code formatter check |
| `run-typecheck` | No | `true` | Run type checking |
| `run-coverage` | No | `false` | Run test coverage reporting |
| `formatter-command` | No | — | Custom formatter command |
| `typecheck-command` | No | — | Custom type check command |
| `coverage-command` | No | — | Custom coverage command |
| `coverage-threshold` | No | `0` | Minimum coverage % to pass |
| `install-command` | No | — | Custom install command |
| `working-directory` | No | `.` | Working directory for commands |

**Secrets:** `codecov-token` — Codecov upload token

### Notify (`notify.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `provider` | Yes | `slack` | `slack`, `discord`, or `both` |
| `status` | No | `success` | `success`, `failure`, `cancelled`, or custom |
| `title` | No | — | Notification title |
| `message` | No | — | Custom message |
| `mention-on-failure` | No | — | Mention on failure (`@channel`, role ID, etc.) |
| `include-commit-info` | No | `true` | Include commit SHA and author |

**Secrets:** `slack-webhook-url` — Slack webhook | `discord-webhook-url` — Discord webhook

### Stale (`stale.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `days-before-stale` | No | `60` | Days of inactivity before stale |
| `days-before-close` | No | `14` | Days after stale before close |
| `stale-issue-message` | No | *(built-in)* | Message for stale issues |
| `stale-pr-message` | No | *(built-in)* | Message for stale PRs |
| `stale-issue-label` | No | `stale` | Label for stale issues |
| `stale-pr-label` | No | `stale` | Label for stale PRs |
| `exempt-issue-labels` | No | `pinned,security,bug` | Labels that prevent stale marking |
| `exempt-pr-labels` | No | `pinned,security` | Labels that prevent stale marking |
| `operations-per-run` | No | `100` | Max operations per run |

### Auto Label (`label.yml`)

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `config-path` | No | `.github/labeler.yml` | Path to labeler config |
| `sync-labels` | No | `false` | Remove labels when files are reverted |
| `dot` | No | `true` | Match dotfiles |
| `use-default-config` | No | `true` | Use built-in rules (frontend, backend, docs, ci, deps, tests, config) |

---

## Usage Examples

### Node.js project (npm)

```yaml
name: CI
on: [push, pull_request]

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: node
      node-version: "20"
      package-manager: npm
```

### Next.js project (pnpm) — full pipeline

```yaml
name: Pipeline
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

  quality:
    uses: groupsmix/shared-workflows/.github/workflows/code-quality.yml@main
    with:
      language: node
      package-manager: pnpm
      run-coverage: true
    secrets:
      codecov-token: ${{ secrets.CODECOV_TOKEN }}

  security:
    uses: groupsmix/shared-workflows/.github/workflows/security.yml@main
    with:
      language: node
      package-manager: pnpm
      run-codeql: true
      codeql-languages: javascript

  build:
    needs: ci
    uses: groupsmix/shared-workflows/.github/workflows/build.yml@main
    with:
      language: node
      package-manager: pnpm
      upload-artifact: true
      artifact-path: .next

  preview:
    needs: build
    if: github.event_name == 'pull_request'
    uses: groupsmix/shared-workflows/.github/workflows/preview.yml@main
    with:
      provider: vercel
      package-manager: pnpm
      vercel-org-id: ${{ vars.VERCEL_ORG_ID }}
      vercel-project-id: ${{ vars.VERCEL_PROJECT_ID }}
    secrets:
      deploy-token: ${{ secrets.VERCEL_TOKEN }}

  label:
    if: github.event_name == 'pull_request'
    uses: groupsmix/shared-workflows/.github/workflows/label.yml@main
    with:
      use-default-config: true
```

### Python project (Poetry)

```yaml
name: CI
on: [push, pull_request]

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: python
      python-version: "3.12"
      package-manager: poetry

  quality:
    uses: groupsmix/shared-workflows/.github/workflows/code-quality.yml@main
    with:
      language: python
      package-manager: poetry
      run-coverage: true

  security:
    uses: groupsmix/shared-workflows/.github/workflows/security.yml@main
    with:
      language: python
      package-manager: poetry
```

### Mixed project (Node.js + Python)

```yaml
name: CI
on: [push, pull_request]

jobs:
  ci:
    uses: groupsmix/shared-workflows/.github/workflows/ci.yml@main
    with:
      language: both
      node-version: "20"
      python-version: "3.12"
      package-manager: npm

  quality:
    uses: groupsmix/shared-workflows/.github/workflows/code-quality.yml@main
    with:
      language: both
      package-manager: npm
```

### Docker build + push to GHCR

```yaml
name: Docker
on:
  push:
    tags: ["v*"]

jobs:
  docker:
    uses: groupsmix/shared-workflows/.github/workflows/docker.yml@main
    with:
      image-name: my-app
      registry: ghcr
      platforms: linux/amd64,linux/arm64
      tag-strategy: semver
      cache: true
```

### Build + Deploy with Slack notification

```yaml
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

  notify:
    needs: deploy
    if: always()
    uses: groupsmix/shared-workflows/.github/workflows/notify.yml@main
    with:
      provider: slack
      status: ${{ needs.deploy.result }}
      mention-on-failure: "@channel"
    secrets:
      slack-webhook-url: ${{ secrets.SLACK_WEBHOOK }}
```

### Release with semantic versioning

```yaml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    uses: groupsmix/shared-workflows/.github/workflows/release.yml@main
    with:
      release-type: semantic-release
      language: node
```

### Auto-close stale issues & PRs

```yaml
name: Stale
on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  stale:
    uses: groupsmix/shared-workflows/.github/workflows/stale.yml@main
    with:
      days-before-stale: 30
      days-before-close: 7
      exempt-issue-labels: "pinned,security,bug"
```

### Custom lint/test commands

```yaml
name: CI
on: [push, pull_request]

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

---

## How It Works

These workflows use [`workflow_call`](https://docs.github.com/en/actions/using-workflows/reusing-workflows) to act as reusable templates. Your repo calls them like a function, passing in the inputs relevant to your project.

**Supported ecosystems:**

- **Node.js** — npm, yarn, pnpm
- **Python** — pip, poetry
- **Frameworks** — Next.js, React, FastAPI, Django, Flask, and anything with standard build/test/lint commands
- **Docker** — any Dockerfile-based project

**Key features:**

- Dependency caching for fast CI runs
- Configurable lint, test, build, and deploy commands
- Artifact upload support for build outputs
- GitHub Environments support for deploy approvals
- Secret passing for deploy tokens, registry credentials, webhooks
- Dependency vulnerability scanning (npm audit, pip-audit)
- CodeQL static analysis
- Automated releases with semantic-release or conventional commits
- Docker multi-platform builds with layer caching
- PR preview deployments (Vercel, Netlify, custom)
- Code formatting (Prettier, Black) and type checking (tsc, mypy)
- Test coverage with Codecov integration
- Slack and Discord notifications
- Auto-stale issue/PR management
- Auto-labeling PRs by file paths

---

## License

MIT
