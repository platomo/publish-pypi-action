# 🛠️ GitHub Action: Publish Python Package

## 📄 Description

The `Publish Python Package` GitHub Action builds and publishes a Python package.
It builds the package, creating both a binary wheel and a source tarball,
and publishes them to the specified PyPi instance.

It needs a GitHub environment and a variable `PYPI_REPO_URL` defined.

## Inputs

| Name      | Description                          | Required | Default |
|-----------|--------------------------------------|:--------:|:-------:|
| `out_dir` | Directory where builds are saved to. |    No    | `dist/` |

## Usage

The repository needs a `pyproject.toml`.

Set up two environments in GitHub and add a publisher on PyPi.

The environment name needs to be set up in PyPi as publisher. We are normally using
two environments for TestPyPi (nightly builds) and PyPi (releases). The environments are
named `testpypi` and `pypi`. The environment `pypi` needs a protection rule so that each
deployment is reviewed manually. It should also just be used by protected branches only.
The environment variable `PYPI_REPO_URL` needs to be defined
(`https://test.pypi.org/legacy/` or `https://upload.pypi.org/legacy/`).

See [this link](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/)
for more information.

Use this action by creating a workflow file, for example:
`.github/workflows/release-testpypi.yml`):

```yaml
name: Publish to TestPyPI

on:
  push:
    branches:
      - main

jobs:
  test:
    uses: './.github/workflows/test.yml'
  regression-test:
    uses: './.github/workflows/regression-test.yml'
    secrets: inherit
  publish-to-testpypi:
    runs-on: ubuntu-latest
    needs:
      - test
      - regression-test
    environment:
      name: testpypi
      url: https://test.pypi.org/p/<package-name>
    permissions:
      id-token: write  # IMPORTANT: mandatory for trusted publishing
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - name: Publish Python Package
        uses: platomo/publish-pypi-action@v1
        with:
          out_dir: /dist
```

## 🆕 Create a new release

To create a new release of the action, use the GitHub release function and create a new
tag according to semantic version requirements (vX.Y.Z).
The GitHub workflow `major-release-tag.yml` will automatically move the major version
tag to the new release.

## ⚖️ License

GNU GENERAL PUBLIC LICENSE Version 3