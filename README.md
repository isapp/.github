# .github

Organization-level GitHub configuration and reusable workflows.

## Reusable Workflows

### Claude Code Review

Automated PR review using Claude AI with security, compliance, and code quality checks.

**Location:** [.github/workflows/claude-review.yml](.github/workflows/claude-review.yml)

#### Setup in Your Repository

1. Create `.github/workflows/claude.yml` in your repo:

```yaml
name: Claude Code – PR Review
on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  review:
    uses: isapp/.github/.github/workflows/claude-review.yml@main
    with:
      model: "claude-sonnet-4-5"
      max_turns: 6
    secrets: inherit
```

2. Copy [CLAUDE.md](./CLAUDE.md) to your repository root for custom review guidelines (optional).

3. Ensure your repository has `ANTHROPIC_API_KEY` secret configured at the organization level.

#### Configuration Options

| Input | Description | Default |
|-------|-------------|---------|
| `model` | Claude model to use | `claude-sonnet-4-5` |
| `max_turns` | Maximum conversation turns | `6` |

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
