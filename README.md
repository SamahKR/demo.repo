name: Step 0, Start

# This step triggers after the learner creates a new repository from the template
# This step sets STEP to 1
# This step closes <details id=0> and opens <details id=1>

# This will run every time we create push a commit to `main`
# Reference https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows
on:
  workflow_dispatch:
  push:
    branches:
      - main

# Reference https://docs.github.com/en/actions/security-guides/automatic-token-authentication
permissions:
  # Need `contents: read` to checkout the repository
  # Need `contents: write` to update the step metadata
  contents: write

jobs:
  on_start:
    name: On start

    # We will only run this action when:
    # 1. This repository isn't the template repository
    # Reference https://docs.github.com/en/actions/learn-github-actions/contexts
    # Reference https://docs.github.com/en/actions/learn-github-actions/expressions
    if: ${{ !github.event.repository.is_template }}

    # We'll run Ubuntu for performance instead of Mac or Windows
    runs-on: ubuntu-latest

    steps:
      # We'll need to check out the repository so that we can edit the README
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0 # Let's get all the branches

      # Update README to close <details id=0> and open <details id=1>
      # and set STEP to '1'
      - name: Update to step 1
        uses: skills/action-update-step@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          from_step: 0
          to_step: 1
          branch_name: my-first-branch
