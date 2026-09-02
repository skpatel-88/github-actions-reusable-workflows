# github-actions-reusable-workflows
A collection of reusable GitHub Actions workflow templates for automating CI/CD, testing, builds, deployments, and other development workflows.

## Slack Notification

`.github/workflows/slack-notification.yml` is a reusable workflow that sends a message to Slack via an incoming webhook.

### Inputs

| Name      | Required | Description                                  |
|-----------|----------|-----------------------------------------------|
| `message` | yes      | Message text to include in the notification    |
| `status`  | no       | Status/result to report (e.g. `success`, `failure`) |

### Secrets

| Name                 | Required | Description                 |
|----------------------|----------|------------------------------|
| `SLACK_WEBHOOK_URL`  | yes      | Slack incoming webhook URL   |

### Usage

Call it from another workflow's job using `uses:`:

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
    uses: <owner>/github-actions-reusable-workflows/.github/workflows/slack-notification.yml@main
    with:
      message: "Build finished"
      status: ${{ needs.build.result }}
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

Replace `<owner>` with the GitHub org/user that owns this repository, and make sure the calling repository has a `SLACK_WEBHOOK_URL` secret set.
