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
