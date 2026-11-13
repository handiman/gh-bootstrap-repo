# gh-bootstrap-repo

A [GitHub CLI](https://cli.github.com/) extension that applies secure-by-default settings to a repository. Template repos copy files, not settings — run this once per new repo.

## Install

```sh
gh extension install handiman/gh-bootstrap-repo
```

## Usage

```sh
gh bootstrap-repo <owner/repo> [--checks "build,test"] [--no-codeql] [--no-actions-allowlist] [--no-require-signed]
```

Typical flow:

```sh
gh repo create my-repo --template handiman/some-template --private
gh bootstrap-repo handiman/my-repo
```

Safe to re-run. Steps the repo's plan or visibility doesn't support are reported as skipped.

## What it sets

**Merging**
- Squash merge only, PR title as commit message
- Delete head branches after merge
- Sign-off required on web-based commits

**Dependencies and secrets**
- Dependabot alerts and security updates
- Secret scanning and push protection
- CodeQL default setup (`--no-codeql` if the repo has its own CodeQL workflow)
- Private vulnerability reporting (public repos)

**GitHub Actions**
- `GITHUB_TOKEN` read-only by default, cannot approve PRs
- Only GitHub-owned and verified-creator actions (`--no-actions-allowlist` to skip)

**Default branch ruleset** (`secure-default-branch`)
- No deletion, no force push
- Pull request required, squash only, review threads resolved
- Signed commits (`--no-require-signed` to skip)
- Required status checks, branch up to date (`--checks`)
- Repository admins may bypass, via pull request only

## Requirements

`gh` (authenticated, admin on the target repo) and `jq`.
