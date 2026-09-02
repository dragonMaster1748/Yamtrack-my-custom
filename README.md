# Yamtrack — Personal Custom Build

This repository is my customized fork of **Yamtrack**, a self-hosted media tracker.

The original Yamtrack project is maintained by FuzzyGrim:

**Original project:** https://github.com/FuzzyGrim/Yamtrack

For the original application's documentation, installation instructions, features, screenshots, support, and development information, use the upstream Yamtrack repository and documentation.

## About this fork

This fork exists to maintain my personal changes on top of Yamtrack without automatically following every upstream release.

The `main` branch is my actively maintained customized version and is the source used for my self-hosted Docker image. The `dev` branch is retained as the original/upstream reference branch.

Updates are deliberately manual and selective. When I decide to adopt a newer upstream Yamtrack release, the custom behavior is reviewed and ported onto the newer code rather than automatically applying every upstream update.

## How this differs from official Yamtrack

The custom features maintained in this fork are documented in [`CUSTOM_CHANGES.md`](CUSTOM_CHANGES.md). The current differences include:

- Cross-account tracking visibility in media search results.
- Current-account-aware tracking labels so an item tracked only by the signed-in account says **In your account** rather than referring to the account as another tracker.
- A visible **Account** indicator in the top navigation showing the username of the account currently signed in.
- A simplified, manual-only GitHub Actions setup for building the personal Docker image.

## Docker image

The customized Docker image is published to GitHub Container Registry as:

```yaml
image: ghcr.io/dragonmaster1748/Yamtrack-my-custom:main
```

The image is built from the `main` branch through the manually triggered **Build Yamtrack Main Image** GitHub Actions workflow. Editing or committing code does not automatically build or deploy an image.

## Branches

- **`main`** — actively maintained customized Yamtrack source and Docker image source.
- **`dev`** — original/upstream reference source.
- **`custom-v...` branches** — historical custom snapshots retained when useful for rollback/reference.

## Updating the custom version

Future upstream updates are intentionally handled manually:

1. Choose an upstream Yamtrack version worth adopting.
2. Compare the new upstream code with the current customized version.
3. Review [`CUSTOM_CHANGES.md`](CUSTOM_CHANGES.md) so each intended customization is preserved.
4. Reimplement or port the custom behavior while preserving upstream changes.
5. Test the customized build.
6. Manually run the GitHub Action to build and publish the `main` Docker image.
7. Deploy the tested image to the self-hosted installation.

The goal is to keep a stable personal build rather than automatically following every upstream release.

## License and upstream attribution

This repository is a fork of [FuzzyGrim/Yamtrack](https://github.com/FuzzyGrim/Yamtrack) and retains the upstream project's **GNU Affero General Public License v3.0 (AGPL-3.0)**.

Yamtrack itself and the majority of this codebase originate from the upstream Yamtrack project. This repository documents the additional personal modifications maintained in this fork.
