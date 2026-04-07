# TypeDoc Report Action

Generates TypeScript documentation in JSON format using [TypeDoc](https://typedoc.org/) and publishes it to a central monitoring repository (`MonitoringTool-DB`).

## Usage

```yaml
name: TypeDoc Report

on:
  push:
    branches: [main]

jobs:
  typedoc:
    runs-on: ubuntu-latest
    steps:
      - uses: ElJijuna/typedoc-action@v1
        with:
          token: ${{ secrets.MONITORING_TOKEN }}
```

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `token` | Yes | GitHub PAT with write access to `MonitoringTool-DB` |

## Output

The action generates a JSON file named `{repository-name}--typedocs.json` and pushes it to the `MonitoringTool-DB` repository under the same owner.

## Setup

### 1. TypeDoc configuration

Make sure your project has a `typedoc.json` at the root so TypeDoc knows the entry point:

```json
{
  "entryPoints": ["src/index.ts"],
  "out": "docs"
}
```

### 2. Create the MONITORING_TOKEN secret

1. Go to [GitHub Settings → Developer settings → Personal access tokens](https://github.com/settings/tokens)
2. Create a token with `repo` scope
3. Add it as a secret named `MONITORING_TOKEN` in your repository:
   **Settings → Secrets and variables → Actions → New repository secret**

## How it works

1. Checks out the repository
2. Runs `npx typedoc --json` to generate documentation
3. Uploads the JSON as a workflow artifact
4. Clones `MonitoringTool-DB` using the provided token
5. Moves the JSON file into the monitoring repo
6. Commits and pushes with message `📚 Update typedoc report`
