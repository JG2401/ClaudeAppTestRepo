# ClaudeAppTestRepo

A test repository demonstrating how to connect Claude to GitHub using the [Claude Code GitHub Action](https://github.com/anthropics/claude-code-action).

## What you get

- **`@claude` mentions**: Mention `@claude` in an issue, issue comment, pull request comment or PR review. Claude analyzes the request, answers questions, and can implement changes and push them to a branch.
- **Automatic PR code review**: Claude reviews every new or updated pull request and posts feedback.

## How Claude is connected to GitHub

### 1. Install the Claude GitHub App

Install the app at <https://github.com/apps/claude> and grant it access to this repository. It needs read/write access to contents, issues, and pull requests.

Alternatively, run `/install-github-app` from within Claude Code in your terminal for a guided setup.

### 2. Add authentication

Add a repository secret under **Settings → Secrets and variables → Actions**:

| Secret | Description |
| --- | --- |
| `CLAUDE_CODE_OAUTH_TOKEN` | OAuth token for your Claude subscription (generate with `claude setup-token`) |

If you use the Anthropic API instead, create an `ANTHROPIC_API_KEY` secret and pass it via `anthropic_api_key` in the workflow.

Never commit tokens or keys to the repository.

### 3. Add the workflows

Two workflows live in [`.github/workflows`](.github/workflows):

- [`claude.yml`](.github/workflows/claude.yml) runs Claude when `@claude` appears in an issue (opened/assigned), issue comment, PR review comment, or PR review.
- [`claude-code-review.yml`](.github/workflows/claude-code-review.yml) runs an automated code review when a pull request is opened, updated, reopened, or marked ready for review.

Both use `anthropics/claude-code-action@v1` and authenticate with `CLAUDE_CODE_OAUTH_TOKEN`.

## Usage

1. Open an issue or comment on a PR and mention `@claude` with your request, e.g. `@claude fix the typo in the README`.
2. Claude posts a progress comment with a checklist and updates it as it works.
3. For code changes, Claude pushes a branch and provides a link to open a pull request.
4. Review and merge as usual.

## Configuration

Customize behavior through the `with:` inputs of the action, for example:

- `prompt`: a fixed instruction instead of the comment text
- `claude_args`: CLI options such as `--allowedTools` or `--max-turns`
- `additional_permissions`: e.g. `actions: read` so Claude can read CI results

See the [usage docs](https://github.com/anthropics/claude-code-action/blob/main/docs/usage.md) and the [FAQ](https://github.com/anthropics/claude-code-action/blob/main/docs/faq.md).

## Limitations

- Claude cannot approve pull requests or submit formal PR reviews.
- Claude cannot modify files in `.github/workflows` (GitHub App permission restriction).
- Claude only responds to users with write access to the repository.

## Troubleshooting

- **No response to `@claude`**: check the **Actions** tab for a failed run and confirm the app is installed and the secret is set.
- **Authentication errors**: regenerate the token and update the secret.
