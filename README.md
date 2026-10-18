![check-version-action header](https://raw.githubusercontent.com/gi-org-pl/check-version-action/main/.github/image/header.png)

# check-version-action

A GitHub Action that fails if the given version tag already exists in the repository, preventing accidental re-use of already published versions.

## Requirements

This action requires a full git history to reliably fetch all tags. Make sure your checkout step has `fetch-depth: 0` set:

```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 0
```

## Usage

```yaml
- name: Check if image version is available
  uses: gi-org-pl/check-version-action@v1
  with:
    version: '1.0.0'
```

## Inputs

| Input     | Description                         | Required |
|-----------|-------------------------------------|----------|
| `version` | Version tag to check (e.g. `1.0.0`) | Yes      |

## Example workflow

```yaml
name: PR Sanity Checks

on:
  pull_request:

jobs:
  check:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Check if image version is available
        uses: gi-org-pl/check-version-action@v1
        with:
          version: ${{ inputs.version }}
```

### Reading version from `package.json`

```yaml
- name: Check if image version is available
  uses: gi-org-pl/check-version-action@v1
  with:
    version: "$(jq -r '.version' package.json)"
```

## How it works

The action checks whether the provided version string already exists as a git tag using `git tag -l`. It also detects shallow clones and fails early with a descriptive error if `fetch-depth: 0` was not set, preventing false-negatives.

## Authors

Action author is [Oskar Barcz](https://github.com/oskarbarcz), CTO of **[Generacja Innowacja](https://gi.org.pl)** —
Poland's first technology-first NGO.

## License

MIT
