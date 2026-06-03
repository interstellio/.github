# .github

Central reusable workflows for the **Interstellio** GitHub organisation.

## Available Workflows

### PR Status Notifications

Sends Slack notifications for pull request lifecycle events.

**Events covered:**

- PR opened
- PR merged (with pull reminder)
- PR closed without merge
- Review requested (user and team)
- Review approved
- Changes requested
- PR ready for review

## PR Status Stub

Add this file to your repo at `.github/workflows/pr-notifications.yml`:

```yaml
name: PR Status Notifications

on:
  pull_request:
    types: [opened, closed, review_requested, ready_for_review]
  pull_request_review:
    types: [submitted]

jobs:
  notify:
    uses: interstellio/.github/.github/workflows/pr-notifications.yml@main
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Direct Push Notifications

Alerts when someone pushes directly to `main` without a pull request.

**Events covered:**

- Direct push to `main` (excludes PR merge commits)

## Direct Push Stub

Add this file to your repo at `.github/workflows/direct-push-notifications.yml`:

```yaml
name: Direct Push Notifications

on:
  push:
    branches: [main]

jobs:
  notify:
    if: >-
      !startsWith(github.event.head_commit.message, 'Merge pull request')
    uses: interstellio/.github/.github/workflows/direct-push-notifications.yml@main
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## Requirements

Each consuming repo must have a `SLACK_WEBHOOK_URL` repository or organisation secret configured.
