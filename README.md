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
