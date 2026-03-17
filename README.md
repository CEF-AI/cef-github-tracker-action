# CEF PR Tracker Action

Reusable GitHub Action that sends PR, review, and push events to the [CEF Project Tazz](https://github.com/cef-ai/Project-Tazz-Execution-Log-Tracker) execution log tracker. One workflow file in any repo, no copy-paste of payload logic.

## Usage

Create `.github/workflows/cef-pr-tracker.yaml` in your repo:

```yaml
name: CEF PR Tracker

on:
  pull_request:
    types: [opened, closed, reopened, synchronize]
  pull_request_review:
    types: [submitted]
  push:
    branches: ['**']

jobs:
  send:
    runs-on: ubuntu-latest
    steps:
      - uses: cef-ai/cef-pr-tracker-action@v1
        with:
          wallet_uri: ${{ secrets.WALLET_URI }}
          notion_api_key: ${{ secrets.NOTION_API_KEY }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          agent_service: '0x2bc3e8a0d6b12e660f519c74697f0929b9b815afc51544bdef08535a5d1c2d4f'
          workspace: '2199'
          stream: 'stream-fd9b5419'
          # Optional:
          wiki_check_branches: 'main,master'
          # ddc_base_url defaults to https://compute-1.devnet.ddc-dragon.com
```

## Required secrets

| Secret | Description |
|--------|-------------|
| `WALLET_URI` | CEF wallet URI for DDC authentication |
| `NOTION_API_KEY` | Notion internal integration token |

`GITHUB_TOKEN` is provided by GitHub Actions; pass it as `github_token` so the action can include it in the payload for the CEF agent.

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `wallet_uri` | Yes | — | CEF wallet URI |
| `notion_api_key` | Yes | — | Notion API key |
| `github_token` | Yes | — | e.g. `${{ secrets.GITHUB_TOKEN }}` |
| `agent_service` | Yes | — | CEF agent service ID |
| `workspace` | Yes | — | CEF workspace ID |
| `stream` | Yes | — | CEF event stream ID |
| `wiki_check_branches` | No | `main,master` | Branches that trigger wiki staleness checks |
| `ddc_base_url` | No | `https://compute-1.devnet.ddc-dragon.com` | DDC compute base URL |

## Pinning a version

Use a tag (e.g. `@v1`, `@v1.0.1`) instead of `@main` so updates are intentional:

```yaml
- uses: cef-ai/cef-pr-tracker-action@v1
```

## Turning this into a repo

1. Create a new repo (e.g. `cef-ai/cef-pr-tracker-action`).
2. Copy the contents of this folder (at least `action.yml` and this README) into the repo root.
3. Commit, push, and create a tag (e.g. `v1`) so others can use `@v1`.
4. In consuming repos, use `uses: cef-ai/cef-pr-tracker-action@v1` (replace with your org/repo and tag).
# cef-pr-tracker-action
