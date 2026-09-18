# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Does

Builds and publishes Docker images (`carepaydev/openjdk`) containing OpenJDK with CI/CD build tools. Images are published to Docker Hub on git tag pushes.

## Image Matrix

Two parallel Dockerfiles produce a full matrix of images:

- **Base OS**: `alpine/` (Alpine 3.x) and `slim/` (Debian Trixie slim)
- **JDK versions**: `nojdk`, `11`, `17`, `21`, `25` — using Amazon Corretto
- **AWS variant**: with or without AWS CLI + ECR credential helper

Published tags follow the pattern: `carepaydev/openjdk:{jdk}-{os}[-aws]`
Examples: `21-alpine`, `17-slim-aws`, `nojdk-alpine`

## Build Commands

Build a specific image locally:
```bash
# Alpine, JDK 21, no AWS
docker build --build-arg JDK_VERSION=21 -t openjdk:21-alpine alpine/

# Slim, JDK 17, with AWS CLI
docker build --build-arg JDK_VERSION=17 --build-arg AWS_CLI_VERSION=2.15.35 -t openjdk:17-slim-aws slim/

# No JDK (tool-only)
docker build --build-arg JDK_VERSION=nojdk -t openjdk:nojdk-alpine alpine/
```

## CI/CD

- **`ci.yml`**: Builds all matrix combinations on every push to non-main branches (no push to registry)
- **`publish.yml`**: Builds and pushes all matrix combinations to Docker Hub on any git tag

To release: push a git tag. Requires `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets in GitHub.

## Architecture Notes

Both `alpine/Dockerfile` and `slim/Dockerfile` share the same `ARG` interface:
- `JDK_VERSION` (default `21`): If set to `nojdk`, the JDK install block is skipped
- `AWS_CLI_VERSION`: If unset/empty, the AWS CLI + ECR config block is skipped

The Alpine variant uses `apk.corretto.aws` for JDK packages and edge/testing repos for `kubectl`. The slim variant uses `apt.corretto.aws` and downloads a static Docker binary directly.

Both variants install `vacuum` (OpenAPI linter) and `yamlfmt` from GitHub releases at pinned versions.

## Pre-commit Hooks

The repo uses pre-commit for linting. Hooks include: Dockerfile linting (hadolint), YAML formatting/validation, trailing whitespace, AWS credential detection.

```bash
pre-commit run --all-files
```
