# PR Agent Action

An automated PR review workflow powered by [PR-Agent](https://github.com/The-PR-Agent/pr-agent).

---

## Table of Contents

- [Setup](#setup)
- [Configuration](#configuration)
- [Usage](#usage)

---

## Setup

### 1. Copy the workflow file

Copy [`template/pr-agent.yml`](template/pr-agent.yml) into the `.github/workflows/` folder of your repository.

```
your-repo/
└── .github/
    └── workflows/
        └── pr-agent.yml
```

The template works out of the box without any modifications.

### 2. Get a Gemini API key

Create an account at [Google AI Studio](https://aistudio.google.com/) and generate a **free** API key.

### 3. Add the API key to your repository secrets

In your repository, go to **Settings → Secrets and variables → Actions** and add a new secret:

- **Name:** `GEMINI_API_KEY`
- **Value:** your API key from the previous step

### 4. Commit to main first

The workflow must be committed to the `main` branch before it can run. Merging it in via a PR will not trigger it on that same PR — it will only take effect on subsequent PRs.

---

## Configuration

The shared workflow (`pr-agent.yml` in this repo) is pre-configured to mimic the review style of Gemini Code Assist. Changes made here will affect **all repositories** using it.

**If you want to customise the configuration for your repo only:**

1. Copy the full workflow from [`.github/workflows/pr-agent.yml`](.github/workflows/pr-agent.yml) into your own repository.
2. Replace the `on:` section with the one found in [`template/pr-agent.yml`](template/pr-agent.yml).
3. Add or override any configuration keys under the `env:` section of the workflow step.

For the full list of available configuration options, refer to the [PR-Agent configuration reference](https://github.com/The-PR-Agent/pr-agent/blob/main/pr_agent/settings/configuration.toml).

> **Note:** The model has been pre-set to `gemini/gemini-3.1-flash-lite-preview` as it offers the highest usage limit under the free tier.

---

## Usage

### Automatic behaviour

Whenever a PR is opened, reopened, or marked as ready for review, the agent will automatically:

- Generate a PR summary (posted as a comment)
- Run code improvement suggestions

### Manual commands (via PR comments)

| Command           | Description                                             |
| ----------------- | ------------------------------------------------------- |
| `/improve`        | Re-runs the code review                                 |
| `/ask <question>` | Ask the agent a question about the code or a suggestion |

### Generating a full PR review

A structured PR review (including estimated effort to review, risk level, etc.) can be generated on demand. This is disabled by default in the shared workflow to avoid an extra LLM call, since `/improve` already covers the core feedback.

To enable it for your repo, set the following in your workflow's `env:` block:

```yaml
github_action_config.auto_review: "true"
```

For a full list of available commands and features, refer to the [PR-Agent documentation](https://docs.pr-agent.ai/tools/).
