# Custom Yamtrack Modifications

This document tracks the intentional differences between this personal build and the official upstream [FuzzyGrim/Yamtrack](https://github.com/FuzzyGrim/Yamtrack).

The purpose is to provide a single checklist of custom behavior that should be reviewed whenever this fork is updated to a newer upstream Yamtrack version.

## Branch and update policy

- `main` is the actively maintained customized version.
- `dev` is the original/upstream reference version.
- Versioned `custom-v...` branches may be retained as historical known-good snapshots for rollback and comparison.
- Upstream releases are not merged automatically.
- Docker images are not built automatically after code changes.
- The Docker image is built manually with the **Build Yamtrack Main Image** GitHub Action when a new image is wanted.
- Current Docker image: `ghcr.io/dragonmaster1748/Yamtrack-my-custom:main`.

## Modification 1 — Cross-account tracking visibility

### Difference from official Yamtrack

Media search results can show which local Yamtrack accounts are already tracking the same media.

Official Yamtrack primarily enriches search results with the current user's own tracking state. This custom build additionally looks up local accounts tracking each search result.

### Custom behavior

- Search results can display the usernames of accounts already tracking the media.
- Cross-account information is available in both grid and list search-result layouts.
- Seasons are identified using their season number so tracking information corresponds to the correct season.
- Tracking usernames are de-duplicated and sorted case-insensitively.
- If the signed-in account is the only account tracking an item, the result displays **In your account** instead of redundantly displaying `Tracked by <current username>`.
- When cross-account information is useful, the tracked-account display remains available.

### Main implementation areas

- `src/app/helpers.py`
- `src/templates/app/search.html`
- `src/templates/app/components/media_card.html`
- `src/templates/app/components/media_card_list.html`

## Modification 2 — Current account indicator

### Difference from official Yamtrack

The main navigation area displays the account currently signed in.

### Custom behavior

- An **Account** label and the current username appear in the top navigation beside the search area.
- The username is taken from the authenticated Yamtrack user.
- The indicator makes it easier to distinguish accounts when using multiple Yamtrack profiles/accounts on the same installation.

### Main implementation area

- `src/templates/base.html`

## Modification 3 — Manual-only Docker build workflow

### Difference from official repository automation

This fork intentionally does not retain the collection of automated test, lint, documentation, CodeQL, stale-issue, image-cleanup, and automatic Docker workflows that are unnecessary for this personal deployment model.

### Custom behavior

- `.github/workflows/` contains one maintained workflow: `custom-docker-image.yml`.
- The workflow runs only through `workflow_dispatch` (manual **Run workflow** action).
- It builds source from the `main` branch.
- It publishes the moving Docker image tag `ghcr.io/dragonmaster1748/Yamtrack-my-custom:main`.
- It also creates a commit-SHA-specific `main-...` image tag for identification/history.
- Committing code alone does not build or deploy Yamtrack.

## Historical custom baseline

The first verified custom build is preserved as:

- Branch: `custom-v0.26.3-dev`
- Upstream release baseline: Yamtrack `v0.26.3`
- Upstream release commit: `76856f9e053e7f59469d1eac0238727263e2adfd`
- Source basis: upstream `dev`, which contained additional post-`v0.26.3` development commits when this custom build was created.
- Verified custom snapshot commit: `7017aa0253fe2a99d81db470aaffb33c277bd72c`
- Status: manually tested successfully on the self-hosted Docker deployment.

The `-dev` suffix indicates that snapshot was based on Yamtrack's development branch after the exact `v0.26.3` release.

## Checklist when updating from official Yamtrack

When adopting a newer upstream version:

1. Preserve any known-good custom snapshot needed for rollback.
2. Update the `dev` reference/base as appropriate.
3. Compare upstream changes against `main` rather than blindly replacing customized files.
4. Reapply and verify every modification documented above.
5. Preserve new upstream behavior wherever possible.
6. Test the customized application manually, especially account switching and media search results.
7. Update this file if a customization is added, removed, or changed.
8. Manually run **Build Yamtrack Main Image** when the code is ready for deployment.
9. Deploy the new `:main` image manually.

## Maintenance rule

Whenever a new personal feature changes official Yamtrack behavior, add it to this document. This file should remain the authoritative list of intentional differences between `main` and official Yamtrack.
