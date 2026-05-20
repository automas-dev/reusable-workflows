# Reusable GitHub Workflows

These workflows and actions contain common tasks in github actions workflows.

**V0 ⟶ V1 Migration**

- `setup_python` action was removed
- `create_tag` action was replaced with `increment_version` action using `tag` input
- `increment_version` input `create_tag` was replaced with `dry-run`

## Actions

### Increment Version

Uses: `automas-dev/reusable-workflows/increment_version`

Versions are incremented based on commit messages. The default action is patch,
but others are available based on keywords.

See [reecetech/version-increment](https://github.com/reecetech/version-increment?tab=readme-ov-file#conventional-commits-semver-with-smarts-)

> - patch: build, chore, ci, docs, fix, perf, refactor, revert, style, test
> - minor: feat
> - major: any of the above keywords followed by a '!' character, or 'BREAKING CHANGE:' in commit body

There is an optional `dry-run` input used to disable the creation of git tags
when running.This is useful in workflows where the next version is needed before
the tag is created (eg. if the version is used in a build but the tag isn't
created until the build passes). The action can be called again with `tag` set
to create the git tag without generating a new version.

**Inputs**

| Name      | Required | Default | Description                                       |
| --------- | -------- | ------- | ------------------------------------------------- |
| `dry-run` | false    | true    | Create a git tag with the new version             |
| `tag`     | false    |         | Explicit value used for new version tag           |
| `aliases` | false    | false   | Create version aliases for major / minor versions |

**Outputs**

| Name              | Description          |
| ----------------- | -------------------- |
| `current-version` | The previous git tag |
| `version`         | The new version      |

Example splitting version increment and tag creation

```yaml
steps:
  - uses: automas-dev/reusable-workflows/increment_version@v1.0
    id: version
    with:
      dry-run: true

  - run: echo Do stuff with ${{ steps.version.outputs.version }}

  - uses: automas-dev/reusable-workflows/increment_version@v1.0
    with:
      tag: ${{ steps.version.outputs.version }}
```

## Workflows

### Increment Version

Uses: `automas-dev/reusable-workflows/.github/workflows/increment_version.yml`

This is a convenience wrapper of the `Increment Version` action.

**Inputs**

| Name      | Required | Default | Description                                       |
| --------- | -------- | ------- | ------------------------------------------------- |
| `dry-run` | false    | true    | Create a git tag with the new version             |
| `tag`     | false    |         | Explicit value used for new version tag           |
| `aliases` | false    | false   | Create version aliases for major / minor versions |

**Outputs**

| Name              | Description          |
| ----------------- | -------------------- |
| `current-version` | The previous git tag |
| `version`         | The new version      |

### Terraform Deploy

Uses: `automas-dev/reusable-workflows/.github/workflows/terraform_deploy.yml`

This workflow needs to inherit secrets.

```yaml
jobs:
  tf:
    uses: automas-dev/reusable-workflows/.github/workflows/terraform_deploy.yml@v1.0
    secrets: inherit
```

Deploy terraform code. Optionally includes increment version. This requires
the pull-request write permission.

```yaml
jobs:
  tf:
    uses: automas-dev/reusable-workflows/.github/workflows/terraform_deploy.yml@v1.0
    permissions:
      contents: write
      pull-requests: write
```

**Inputs**

| Name                | Required | Default   | Description                                               |
| ------------------- | -------- | --------- | --------------------------------------------------------- |
| `git-tag`           | false    |           | Git version tag type                                      |
| `increment-version` | false    | true      | Increment version tag instead of using git-tag input type |
| `working-directory` | false    | terraform | Location of terraform code type                           |

**Repo Variables**

These vars need to be available in the github repo.

| Name                 | Description                         |
| -------------------- | ----------------------------------- |
| `TFSTATE_BUCKET`     | R2 bucket name                      |
| `TFSTATE_ACCOUNT_ID` | Cloudflare account id for R2 bucket |

**Repo Secrets**

These secrets need to be available in the github repo.

| Name                 | Description                            |
| -------------------- | -------------------------------------- |
| `TFSTATE_ACCESS_KEY` | R2 bucket access key for tf state file |
| `TFSTATE_SECRET_KEY` | R2 bucket secret key for tf state file |

**Outputs**

These are the same outputs from increment_version. They will be populated
regardless of the `increment-version` input and will not be altered by the
`git-tag` input.

| Name              | Description          |
| ----------------- | -------------------- |
| `current-version` | The previous git tag |
| `version`         | The new version      |

## How to use actions and workflows

Reusable actions and workflows are included using the github user / repo names
followed by a path to a file in the repo. Actions use a path to the folder
containing `action.yml` while workflows use a path to the workflow yaml file.

### Action

In this example, the file `increment_version/action.yml` is being used from the
`automas-dev/reusable-workflows` repo using branch `main`. Tags can also be
instead of a branch name.

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
      - uses: actions/checkout@v6

      - uses: automas-dev/reusable-workflows/increment_version@v1.0
```

### Workflow

In this example, the file `.github/workflows/increment_version.yml` is being
used from the `automas-dev/reusable-workflows` repo using branch `main`. Tags
can also be instead of a branch name.

`.github/workflows/ci.yml`

```yaml
on:
  push:

jobs:
  increment_version:
    uses: automas-dev/reusable-workflows/.github/workflows/increment_version.yml@v1.0
    secrets: inherit

    permissions:
      contents: write
```
