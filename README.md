# Merge Mate GitHub Actions Template

This repository contains a minimal GitHub Actions setup for running GitKraken Merge Mate against pull requests. It replaces the placeholder project copy with documentation for the two workflows that are actually present in the repo.

## What This Repository Includes

- `merge-mate.yml` to keep Merge Mate in sync on pushes to your default branch
- `merge-mate-review.yml` to apply or undo Merge Mate review actions for a pull request
- A lightweight starting point for testing or adapting the official `gitkraken/merge-mate-action` workflows

## Workflows At A Glance

### Merge Mate Sync

The sync workflow runs on pushes to `main` and calls `gitkraken/merge-mate-action/sync@v0.2`. It is intended to keep Merge Mate aligned with the latest state of your default branch.

### Merge Mate Review

The review workflow supports two entry points:

- `workflow_dispatch` for manually running `apply` or `undo` against a pull request number
- `issue_comment` edits on pull requests, excluding bot users

The job calls `gitkraken/merge-mate-action/review@v0.2` with the provided pull request number and action.

## Setup

### Prerequisites

Before enabling these workflows, make sure you have:

- a repository with GitHub Actions enabled
- a default branch named `main`, or an updated trigger in `merge-mate.yml`
- a repository secret named `GK_AI_PROVISIONER_TOKEN`

### Install The Workflows

1. Copy `merge-mate.yml` and `merge-mate-review.yml` into `.github/workflows/` in the target repository.
2. Add the `GK_AI_PROVISIONER_TOKEN` secret in your repository settings.
3. Review the branch trigger in `merge-mate.yml` and replace `main` if your default branch uses a different name.
4. Commit and push the workflow files.

## Usage

### Automatic Sync

Every push to the configured default branch triggers the sync workflow:

```yaml
on:
  push:
    branches: [main]
```

If your repository uses a different branch, update that value before rollout.

### Manual Review Actions

Use the **Merge Mate Review** workflow from the GitHub Actions UI when you need to target a specific pull request manually. Supply:

- `pr-number` with the pull request number
- `action` as either `apply` or `undo`

### Comment-Driven Review Runs

The review workflow also listens for edited comments on pull requests:

```yaml
on:
  issue_comment:
    types: [edited]
```

That lets repository collaborators trigger the review job from PR discussion activity without changing the workflow definition.

## Customization Notes

Both workflows currently request these GitHub permissions:

- `contents: write`
- `pull-requests: write`
- `id-token: write`

Keep those permissions aligned with the requirements of the `gitkraken/merge-mate-action` version you are using. If you upgrade from `v0.2`, review the action documentation before changing scopes or inputs.

The workflows also define concurrency groups so repeated runs for the same branch or pull request do not stack indefinitely:

- sync jobs cancel any in-progress run for the same ref
- review jobs keep prior runs active for the same pull request

Adjust those defaults if your team needs stricter serialization or more aggressive cancellation behavior.

## Repository Layout

```text
.
|-- README.md
|-- merge-mate-review.yml
`-- merge-mate.yml
```

For production use, place the workflow files under `.github/workflows/`.

## Maintenance

- Update the branch trigger any time the repository default branch changes.
- Rotate `GK_AI_PROVISIONER_TOKEN` according to your normal secret management policy.
- Revisit workflow behavior when upgrading `gitkraken/merge-mate-action` from `v0.2`.
- Test manual dispatch behavior after changing workflow inputs or permissions.

## Contributing

Open a pull request with proposed changes to the workflow examples or documentation. Keep README updates tied to the checked-in workflow files so the docs do not drift from the actual configuration.
