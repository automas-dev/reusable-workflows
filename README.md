# Reusable GitHub Workflows

These workflows and actions contain common tasks in github actions workflows.

## Actions

### Increment Version

Uses: `automas-dev/reusable-workflows/increment_version`

Versions are incremented based on commit messages. The default action is patch,
but others are available based on keywords.

See [reecetech/version-increment](https://github.com/reecetech/version-increment?tab=readme-ov-file#conventional-commits-semver-with-smarts-)

> - patch: build, chore, ci, docs, fix, perf, refactor, revert, style, test
> - minor: feat
> - major: any of the above keywords followed by a '!' character, or 'BREAKING CHANGE:' in commit body

There is an optional `create-tag` input used to disable the creation of git tags
when running.This is useful in workflows where the next version is needed before
the tag is created (eg. if the version is used in a build but the tag isn't
created until the build passes).

Tag creation can also be disabled in the commit message. Include `NO RELEASE` in
the commit message to prevent a tag from being created for that commit.

**Inputs**

| Name         | Required | Default | Description                           |
| ------------ | -------- | ------- | ------------------------------------- |
| `create-tag` | false    | true    | Create a git tag with the new version |

**Outputs**

| Name              | Description          |
| ----------------- | -------------------- |
| `current-version` | The previous git tag |
| `version`         | The new version      |

### Create Tag

Uses: `automas-dev/reusable-workflows/create_tag`

If the `increment-version` action is used with `create-tags: false`, this action
can be used to create the version tag later in the workflow. This allows a
workflow to delay tag creation until after steps that need the new version but
may fail.

**Inputs**

| Name  | Required | Default | Description |
| ----- | -------- | ------- | ----------- |
| `tag` | true     |         | Tag value   |

### Setup Python

Uses: `automas-dev/reusable-workflows/setup_python`

Install python and poetry.

**Inputs**

| Name             | Required | Default | Description               |
| ---------------- | -------- | ------- | ------------------------- |
| `python-version` | true     | 3.13    | Python version to install |
| `poetry-version` | true     | 1.8.5   | Poetry version to install |

**Outputs**

| Name              | Description         |
| ----------------- | ------------------- |
| `current-version` | The current version |
| `version`         | The new version     |

>[!WARNING] Deprecated
> This action will be removed in a future version.
>
> Reason: The two steps are so simple it's not worth having an entire action to
> combine them.

## Workflows

### Increment Version

Uses: `automas-dev/reusable-workflows/.github/workflows/increment_version.yml`

Versions are incremented based on commit messages. The default action is patch,
but others are available based on keywords.

This is a convenience wrapper of the `Increment Version` action.

**Inputs**

| Name         | Required | Default | Description                           |
| ------------ | -------- | ------- | ------------------------------------- |
| `create-tag` | false    | true    | Create a git tag with the new version |

**Outputs**

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
      - uses: actions/checkout@v4

      - uses: automas-dev/reusable-workflows/increment_version@v0.1

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
    uses: automas-dev/reusable-workflows/.github/workflows/increment_version.yml@v0.1
    secrets: inherit

    permissions:
      contents: write
```

### ~~Versions~~

~~Previously versions other than main were specified as an exact version. With~~
~~the updated `create-aliases` input of increment_version, workflow and action~~
~~version aliases (eg. `v1` or `v1.3`) are available and recommended.~~
