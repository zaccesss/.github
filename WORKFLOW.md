# Workflow

How I work in my repositories, so the history stays clean and nothing lands broken. This is my default, stamped into new repositories and kept consistent across the fleet.

## Branch, PR, merge

I never commit straight to `main`. I branch off the latest `main`, make one focused change, open a pull request and let it merge itself.

I name every branch for what it does:

- `feat/<short-description>` a new capability
- `fix/<short-description>` a bug fix
- `chore/<short-description>` maintenance, dependencies, config
- `docs/<short-description>` documentation only
- `ci/<short-description>` a workflow or CI change

The flow I follow every time:

```bash
git checkout main && git pull
git checkout -b chore/short-description
# make the change, and add an entry under [Unreleased] in CHANGELOG.md if the repo keeps one
git add -A
git commit -m "chore: short description of what changed"
git push -u origin chore/short-description
gh pr create --title "chore: short description" --body "What changed and why."
gh pr merge --squash --delete-branch --auto
```

- Auto-merge waits for any required check to pass, then squash merges and deletes the branch, so I never sit and watch it.
- One change is one branch is one PR. I keep unrelated work apart.

## Dependencies and maintenance

Dependabot keeps dependencies and action versions current. The rest is handled centrally, not by a workflow in the repo: Dependabot PR auto-merge under my ecosystem rule, keeping open PR branches up to date, deleting merged branches and marking stale issues and branches. So a repo carries no per-repo automerge, branch-update or stale workflow of its own.

## Commits

- Conventional prefixes: `feat`, `fix`, `chore`, `ci`, `docs`.
- Present tense, one clear change per commit.

## Before a PR

- Run whatever check the repo has locally first (lint, build, tests).
- I never commit a secret. A gitleaks scan runs on every PR.
