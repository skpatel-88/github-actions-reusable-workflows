# GitHub Actions Reusable Workflows

A collection of reusable [GitHub Actions workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) for CI/CD, testing, builds, deployments, notifications, and other common automation tasks.

Anyone can call these workflows from their own repository with a single `uses:` line — no need to copy/paste or maintain the same YAML in every project.

## Table of contents

- [Available workflows](#available-workflows)
  - [Slack Notification](#slack-notification)
- [Quick start](#quick-start)
- [Contributing](#contributing)
- [License](#license)

## Available workflows

| Workflow | File | Description |
|----------|------|-------------|
| Slack Notification | [`.github/workflows/slack-notification.yml`](.github/workflows/slack-notification.yml) | Sends a message to a Slack channel via an incoming webhook |

More workflows will be added over time — each one gets its own section below and a row in the table above.

---

### Slack Notification

Sends a message to Slack using an [incoming webhook](https://api.slack.com/messaging/webhooks).

**File:** [`.github/workflows/slack-notification.yml`](.github/workflows/slack-notification.yml)

#### Inputs

| Name      | Required | Type   | Default | Description                                          |
|-----------|----------|--------|---------|-------------------------------------------------------|
| `message` | yes      | string | —       | Message text to include in the notification            |
| `status`  | no       | string | `""`    | Status/result to report (e.g. `success`, `failure`)    |

#### Secrets

| Name                | Required | Description                 |
|---------------------|----------|------------------------------|
| `SLACK_WEBHOOK_URL` | yes      | Slack incoming webhook URL   |

#### Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      result: ${{ steps.build.outcome }}
    steps:
      - id: build
        run: echo "build steps here"

  notify:
    needs: build
    uses: skpatel-88/github-actions-reusable-workflows/.github/workflows/slack-notification.yml@main
    with:
      message: "Build finished"
      status: ${{ needs.build.result }}
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

Make sure the calling repository has a `SLACK_WEBHOOK_URL` secret set (Settings → Secrets and variables → Actions).

> Tip: pin `@main` to a tagged release (e.g. `@v1`) once this repo starts tagging versions, so your workflows don't break from unreleased changes.

## Quick start

1. Pick a workflow from the [table above](#available-workflows).
2. Copy the example `uses:` block from that workflow's section into your own repository's workflow file.
3. Add any required secrets to your repository.
4. Commit and push — your workflow will now call the shared, versioned logic from this repo.

## Contributing

New reusable workflows are welcome:

1. Add the workflow file under `.github/workflows/`, triggered with `on: workflow_call`.
2. Document its `inputs`, `secrets`, and a usage example in this README.
3. Open a pull request.

## License

No license file has been added yet. Until one is added, all rights are reserved by the repository owner.

