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

      - uses: twh2898/reusable-workflows/increment_version@main
```

### Workflow

`.github/workflows/ci.yml`

```yaml
on:
  push:

jobs:
  increment_version:
    uses: twh2898/reusable-workflows/.github/workflows/increment_version.yml@main
    secrets: inherit

    permissions:
      contents: write
```

## Increment Version (workflow)

Uses: `twh2898/reusable-workflows/.github/workflows/increment_version.yml@main`

### Inputs

| Name         | Required | Default | Description                       |
| ------------ | -------- | ------- | --------------------------------- |
| `create-tag` | false    | true    | Create a tag with the new version |

### Outputs

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |

## Increment Version (action)

Uses: `twh2898/reusable-workflows/increment_version@main`

### Inputs

| Name         | Required | Default | Description                       |
| ------------ | -------- | ------- | --------------------------------- |
| `create-tag` | false    | true    | Create a tag with the new version |

### Outputs

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |

## Setup Python

Uses: `twh2898/reusable-workflows/setup_python@main`

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
