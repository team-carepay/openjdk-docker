# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

This project does not follow Semantic Versioning. Releases are sequential `v1.N`
git tags; pushing a tag builds and publishes the full image matrix to Docker Hub
(see `.github/workflows/publish.yml`). Entries below are grouped under the tag
that shipped them, and cover both the published images and the repository's own
tooling.

## [Unreleased]

### Added

- Smoke-test every image built by CI. `ci.yml` now loads the built image into
  the runner (`load: true`) and runs each bundled tool's version command —
  `bash`, `curl`, `git`, `jq`, `docker`, `kubectl`, `yq`, `vacuum` and
  `yamlfmt`, plus `java` unless `JDK_VERSION=nojdk` and `aws` when
  `AWS_CLI_VERSION` is set — so a tool that fails to install breaks the build
  instead of reaching a release

## [1.35] - 2026-09-18

### Added

- Add this `CHANGELOG.md`, following [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
- Add `CLAUDE.md` documenting the image matrix, build commands and release flow
- Lint the GitHub Actions workflows with
  [actionlint](https://github.com/rhysd/actionlint) v1.7.12 in pre-commit
- Add the `end-of-file-fixer`, `mixed-line-ending` and `check-added-large-files`
  pre-commit hooks
- Add `.hadolint.yaml`, suppressing DL3018/DL3008 (pinning every apk/apt package
  defeats the point of a rolling CI toolbox image) and DL4006 (the single long
  `RUN` chain is intentional)

### Changed

- Lint Dockerfiles with [hadolint](https://github.com/hadolint/hadolint) v2.15.1
  instead of `dockerfilelint`, which had been unmaintained upstream since 2018
- Upgrade the pre-commit hook pins: `pre-commit/pre-commit-hooks` v2.3.0 →
  v6.0.0, `macisamuele/language-formatters-pre-commit-hooks` v1.6.1 → v2.16.0,
  and `Lucas-C/pre-commit-hooks` v1.1.9 → v1.5.6
- Guard `main` rather than `master` in the `no-commit-to-branch` hook, and allow
  multi-document YAML plus markdown hard line breaks
- Fetch the Corretto signing key with `curl -fsSL -o` instead of `wget` in the
  Alpine image, so it no longer pulls in a second HTTP client. Behaviour is
  unchanged
- **Breaking (contributors only):** `pre-commit run` now requires a running
  Docker daemon, because the hadolint and actionlint hooks run as container
  images. Published images are unaffected

### Removed

- Remove the `pryorda/dockerfilelint-precommit-hooks` pre-commit hook

### Security

- Upgrade the Alpine base image to 3.24.2, shipping OpenSSL 3.5.8-r0 in the
  `*-alpine` images. This closes CVE-2026-14456, CVE-2026-14457, CVE-2026-18798,
  CVE-2026-54874, CVE-2026-63072, CVE-2026-63073, CVE-2026-63074, CVE-2026-63075,
  CVE-2026-63076 and CVE-2026-75803, and refreshes `ca-certificates-bundle` to
  20260909-r0 and `apk-tools` to 3.0.8-r0. Rebuilding alone does not pick these
  up — `apk add --no-cache` never refreshes the base image's pre-baked packages,
  so `ARG ALPINE_VERSION` is the only lever that patches OpenSSL here.

## [1.34] - 2026-06-16

### Changed

- Upgrade the Alpine base image from 3.23.4 to 3.24.1

## [1.33] - 2026-04-16

### Changed

- Upgrade the Alpine base image from 3.23.3 to 3.23.4
- Document the published tag matrix more clearly in the README

## [1.32] - 2026-04-12

### Fixed

- Restore the Debian slim build by dropping the `pkgs.k8s.io` apt repository;
  `kubectl` now comes from the Debian testing repository instead
- Default `JDK_VERSION` to `21` in the slim image, so `docker build slim/` works
  without passing a build arg

## [1.31] - 2026-04-12

### Fixed

- Install `aws-cli` from the stable Alpine repository by moving the AWS block
  ahead of the `edge/testing` and `edge/community` repositories, instead of
  letting it resolve against edge

## [1.30] - 2026-04-12

### Changed

- Upgrade `vacuum` from 0.17.11 to 0.25.8 and `yamlfmt` from 0.17.2 to 0.21.0 in
  both images
- Default `JDK_VERSION` to `21` in the Alpine image

## [1.29] - 2026-01-28

### Changed

- Upgrade the Alpine base image from 3.23.2 to 3.23.3

## [1.28] - 2025-12-18

### Changed

- Upgrade the Alpine base image from 3.22.1 to 3.23.2
- List the JDK 25 tags in the README image matrix

## [1.27] - 2025-10-09

### Added

- Build and publish JDK 25 images

### Removed

- Drop JDK 22 from the build matrix

## [1.26] - 2025-09-06

### Added

- Install `vacuum` (OpenAPI linter) and `yamlfmt` in both images

## [1.25] - 2025-09-02

### Added

- Configure the ECR credential helper in `~/.docker/config.json` for the AWS
  image variants, so `docker` picks up `ecr-login` without extra setup

### Changed

- Upgrade the Alpine base image from 3.20.3 to 3.22.1
- Move the slim image from `debian:bookworm-slim` to `debian:trixie-slim`

## Earlier releases

Releases before v1.25 (v1.1 through v1.24, 2021-2024) predate this changelog.
See the git history for what they contained:

```bash
git log --oneline v1.1..v1.25
```

[unreleased]: https://github.com/team-carepay/openjdk-docker/compare/v1.35...HEAD
[1.35]: https://github.com/team-carepay/openjdk-docker/compare/v1.34...v1.35
[1.34]: https://github.com/team-carepay/openjdk-docker/compare/v1.33...v1.34
[1.33]: https://github.com/team-carepay/openjdk-docker/compare/v1.32...v1.33
[1.32]: https://github.com/team-carepay/openjdk-docker/compare/v1.31...v1.32
[1.31]: https://github.com/team-carepay/openjdk-docker/compare/v1.30...v1.31
[1.30]: https://github.com/team-carepay/openjdk-docker/compare/v1.29...v1.30
[1.29]: https://github.com/team-carepay/openjdk-docker/compare/v1.28...v1.29
[1.28]: https://github.com/team-carepay/openjdk-docker/compare/v1.27...v1.28
[1.27]: https://github.com/team-carepay/openjdk-docker/compare/v1.26...v1.27
[1.26]: https://github.com/team-carepay/openjdk-docker/compare/v1.25...v1.26
[1.25]: https://github.com/team-carepay/openjdk-docker/compare/v1.24...v1.25
