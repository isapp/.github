# .github

Organization-level GitHub configuration and reusable workflows.

## Claude Code Review Setup

Automated PR review using Claude AI with security, compliance, and code quality checks.

### Setup in Your Repository

1. Create `.github/workflows/claude-review.yml` in your repo:

```yaml
name: Claude Code – PR Review
on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

permissions:
  contents: write
  pull-requests: write
  issues: write
  actions: read
  id-token: write

jobs:
  review:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - name: Checkout PR
        uses: actions/checkout@v5
        with:
          fetch-depth: 0
          ref: ${{ github.event.pull_request.head.sha }}

      - name: Run Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            /review

            IMPORTANT: Also reference and apply the organization-wide code review guidelines:
            https://github.com/isapp/.github/blob/main/CLAUDE.md

            Apply org-wide standards (security, HIPAA, compliance) IN ADDITION to repo-specific CLAUDE.md.
          claude_args: "--model claude-sonnet-4-5 --max-turns 6"
          track_progress: true
```

2. (Optional) Copy [CLAUDE.md](./CLAUDE.md) to your repository root to customize review guidelines for your specific repo.

3. Ensure `ANTHROPIC_API_KEY` secret is configured at the organization level.

### Configuration Options

You can customize the workflow by modifying:

| Parameter | Description | Default | Location |
|-----------|-------------|---------|----------|
| `--model` | Claude model to use | `claude-sonnet-4-5` | `claude_args` |
| `--max-turns` | Maximum conversation turns | `6` | `claude_args` |
| `timeout-minutes` | Workflow timeout | `15` | job level |

#### Review Guidelines

The workflow automatically reads `CLAUDE.md` from your repository root. See [CLAUDE.md](./CLAUDE.md) for the comprehensive template covering:

- Security (OWASP, auth, secrets, PII)
- Compliance (HIPAA, SOC2, FedRAMP)
- Data & migrations
- Mobile (iOS/Android)
- Infrastructure (Terraform, Docker)
- Performance, testing, APIs, dependencies

#### How It Works

1. Workflow triggers on PR events
2. Claude reviews code changes using guidelines from `CLAUDE.md`
3. Posts inline comments on specific lines of code
4. Creates a summary comment with all findings
5. Prioritizes must-fix issues (security, compliance) over suggestions

#### Troubleshooting

**Error: "Invalid input, mode is not defined"**
- Remove the `mode` parameter from your caller workflow (deprecated as of latest version)

**No comments appearing**
- Verify `ANTHROPIC_API_KEY` secret is configured
- Check workflow permissions include `pull-requests: write` and `contents: write`
- Ensure workflow is triggered on correct PR events

**Workflow not running**
- Check that `.github/workflows/claude.yml` exists in your repo
- Verify branch protection rules don't block the workflow
