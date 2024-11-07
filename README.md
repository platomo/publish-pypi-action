# Publish Python Package

This GitHub Action builds and publishes a Python package to TestPyPI.

## Description

This Action is useful for deploying Python packages to TestPyPI to test them before releasing on the official PyPI repository. It builds the package, creating both a binary wheel and a source tarball, and publishes them to the specified TestPyPI instance.

## Inputs

| Name             | Description                        | Required | Default |
| ---------------- | ---------------------------------- | -------- | ------- |
| `repository-url` | The URL of the TestPyPI repository | Yes      | None    |

## Usage

Create a workflow file (e.g., `.github/workflows/publish.yml`) and use this Action as follows:

```yaml
name: Publish to TestPyPI

on:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Publish Python Package
        uses: platomo/publish-pypi-action@main
        with:
          repository-url: https://test.pypi.org/legacy/
```

## Workflow Steps

1. **Install pypa/build**: Installs the `build` package, required for building the Python package.
2. **Build Wheel and Source Tarball**: Builds the package and creates both `.whl` and `.tar.gz` files in the `dist/` folder.
3. **Publish Package to TestPyPI**: Uses [gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish) to upload the package files to the specified TestPyPI repository.

## Required Permissions

To successfully run the Action, an authentication token must be stored in the repository secrets (`secrets`) to publish the package to TestPyPI.
