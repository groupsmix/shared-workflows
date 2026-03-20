# Contributing to shared-workflows

Thanks for your interest in contributing! Here's how to get started.

## Adding a New Workflow

1. Create a new `.yml` file in `.github/workflows/`
2. Use `workflow_call` as the trigger so it can be reused by other repos
3. Add configurable `inputs` with sensible defaults
4. Add `secrets` if the workflow needs credentials
5. Update `README.md` with:
   - A row in the Workflows table
   - An Inputs Reference section
   - At least one Usage Example

## Workflow Guidelines

- **Use latest stable action versions** (e.g., `actions/checkout@v4`)
- **Add descriptions** to all inputs and secrets
- **Provide defaults** wherever possible so callers need minimal configuration
- **Support multiple ecosystems** — if a workflow can work for both Node.js and Python, add a `language` input
- **Cache dependencies** to speed up CI runs
- **Use `continue-on-error`** only when the step is advisory (e.g., audit warnings)

## Testing Changes

Since these are reusable workflows (triggered by `workflow_call`), you can't test them directly. To test:

1. Fork or create a test repo
2. Reference your branch instead of `@main`:
   ```yaml
   uses: groupsmix/shared-workflows/.github/workflows/ci.yml@your-branch
   ```
3. Trigger the workflow and verify it works

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat: add new workflow for X`
- `fix: correct caching in ci.yml`
- `docs: update README examples`
- `ci: bump action versions`

## Pull Requests

- Keep PRs focused on a single workflow or change
- Include a description of what changed and why
- Add or update usage examples in the README
