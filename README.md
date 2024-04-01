# docs-build-static-action GitHub action

This GitHub action does two things:
- Builds project documentation using [@diplodoc/cli](https://github.com/diplodoc-platform/cli).
- Uploads the build files, logs, and inputs as artifacts.

## Inputs

- `src-root` (default: `./`) - The root directory where the source documentation files are located. The action will use this directory as the base location to look for the source files that need to be built.
- `build-root` (default: `./_docs-build`) - The root directory for the built documentation. This is an optional parameter, and if not specified, the default value will be used.

## Usage

Create a file named `.github/workflows/build.yml` in your repo.
This workflow is triggered every time a pull request is opened or updated.

```yaml
name: Build

on:
  workflow_dispatch:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build-docs:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    permissions: write-all
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Build
        uses: diplodoc-platform/docs-build-static-action@v1
        with:
          src-root: "./docs"
```