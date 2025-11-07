# Reusable GitHub Workflows

## Usage

### Action

`.github/workflows/ci.yml`

```yaml
on:
  push:

jobs:
  increment_version:
    runs-on: ubuntu-latest

    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4

      - uses: automas-dev/reusable-workflows/increment_version@main

```

### Workflow

`.github/workflows/ci.yml`

```yaml
on:
  push:

jobs:
  increment_version:
    uses: automas-dev/reusable-workflows/.github/workflows/increment_version.yml@main
    secrets: inherit

    permissions:
      contents: write
```

## Increment Version (workflow)

Uses: `automas-dev/reusable-workflows/.github/workflows/increment_version.yml@main`

See [reecetech/version-increment](https://github.com/reecetech/version-increment?tab=readme-ov-file#conventional-commits-semver-with-smarts-) for usage.

> - patch: build, chore, ci, docs, fix, perf, refactor, revert, style, test
> - minor: feat
> - major: any of the above keywords followed by a '!' character, or 'BREAKING CHANGE:' in commit body

To stop this workflow from running, include `NO RELEASE` in the commit message.

### Inputs

| Name                | Required | Default | Description                                        |
| ------------------- | -------- | ------- | -------------------------------------------------- |
| `create-tag`        | false    | true    | Create a tag with the new version                  |

### Outputs

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |

## Increment Version (action)

Uses: `automas-dev/reusable-workflows/increment_version@main`

### Inputs

| Name                | Required | Default | Description                                        |
| ------------------- | -------- | ------- | -------------------------------------------------- |
| `create-tag`        | false    | true    | Create a tag with the new version                  |

### Outputs

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |

## Create Tag (action)

Uses: `automas-dev/reusable-workflows/create_tag@main`

If `increment-version` is used with `create-tags: false`, this action can be
used to create the version tag. This allows a workflow to delay tag creation
until after steps that need the new version but may fail before tag creation.

### Inputs

| Name                | Required | Default | Description                                        |
| ------------------- | -------- | ------- | -------------------------------------------------- |
| `tag`               | true     |         | Tag value                                          |

## Setup Python (action)

Uses: `automas-dev/reusable-workflows/setup_python@main`

### Inputs

| Name             | Required | Default | Description               |
| ---------------- | -------- | ------- | ------------------------- |
| `python-version` | true     | 3.13    | Python version to install |
| `poetry-version` | true     | 1.8.5   | Poetry version to install |

### Outputs

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |
