# GitHub Action: Automatic Pull Request Creation

This GitHub Action automatically creates a pull request when code is pushed to any branch except `main`. It allows you to automate the creation of pull requests based on your changes, streamlining the process for review and collaboration.

## Workflow Overview

The workflow triggers on every `push` event, ignoring pushes to the `main` branch. It automatically creates a pull request from the pushed branch to `main`, and it can add reviewers, labels, and assignees as configured.

### Features

- **Automated PR Creation:** Automatically creates a pull request from the pushed branch to the `main` branch.
- **Customization:** Set custom commit messages, PR titles, and PR descriptions.
- **Reviewers and Assignees:** Optionally add reviewers and assignees to the pull request.
- **Labels:** Automatically add labels to the created pull request.

## How It Works

1. **Trigger:** The workflow is triggered by a push to any branch except `main`.
2. **Checkout Code:** The repository code is checked out to access the latest changes.
3. **Create Pull Request:** The `peter-evans/create-pull-request` action is used to generate a new pull request with the following configurations:
   - **Commit Message:** `"Auto-generated commit"`
   - **PR Title:** `"Auto-generated PR"`
   - **PR Description:** `"This PR is auto-generated"`
   - **Target Branch:** `main`
   - **Source Branch:** Automatically uses the branch where the push occurred.

### Workflow File Example

This is the configuration of the workflow located in `.github/workflows/create-pr.yml`:

```yaml
name: create-pr

on:
  push:
    branches-ignore:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  create-pr:
    name: Create Pull Request
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository code
        uses: actions/checkout@v4
      
      - name: Create Pull Request
        uses: peter-evans/create-pull-request@v7
        with:
          commit-message: "Auto-generated commit"
          title: "Auto-generated PR"
          body: "This PR is auto-generated"
          branch: "auto-generated-branch"
          base: "main"
          labels: "automated-pr"
          reviewers: "reviewer1"
          assignees: "assignee1"
```

## Customization

You can modify the following options in the workflow file:

- **Commit Message**: Set a custom message for the commit made by the Action.
- **PR Title & Description**: Customize the title and body of the created pull request.
- **Branch**: Change the target branch or the naming pattern of the auto-generated branch.
- **Labels**: Assign custom labels to your pull request.
- **Reviewers & Assignees**: Automatically request specific users to review or be assigned to the PR.

## Usage

1. Ensure that you have this workflow file located in `.github/workflows/create-pr.yml`.
2. When you push changes to a branch (excluding `main`), this action will trigger.
3. After the push, a pull request will be automatically created targeting the `main` branch.
4. Reviewers and assignees will be notified, and the pull request will include any configured labels.

## Resources

- [peter-evans/create-pull-request](https://github.com/peter-evans/create-pull-request) – Documentation for the action used to create pull requests.