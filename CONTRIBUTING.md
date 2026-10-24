# Contributing to ci-images

Thank you for your interest in contributing. ci-images holds shared CI container images for AGLFlorida repos - each image is a directory under `images/` with a `Dockerfile`, `VERSION`, and `README.md`.

## Reporting bugs

Open a [GitHub issue](https://github.com/AGLFlorida/ci-images/issues/new) and include:

- Which image (`images/<name>`)
- Steps to reproduce (the workflow run, or local `docker build` command)
- What you expected vs. what happened

## Suggesting features

Open an issue with the `enhancement` label. Check [existing issues](https://github.com/AGLFlorida/ci-images/issues) first to avoid duplicates.

## Submitting a pull request

1. Fork the repo and create a branch from `main`.
2. Add or edit an image directory under `images/`, or update the shared workflows.
3. Bump the image's `VERSION` file if the Dockerfile's pinned tool version changed.
4. Test locally (see below).
5. Open a PR against `main` with a clear description of what changed and why.

Keep PRs focused - one logical change per PR. If you're unsure whether a change is in scope, open an issue first.

## Testing

```sh
# Build an image locally
docker build -t ci-<image-name>:test images/<image-name>

# Lint a Dockerfile
docker run --rm -i hadolint/hadolint < images/<image-name>/Dockerfile

# Lint workflow files
actionlint
```

CI runs hadolint, actionlint, and a Trivy vulnerability scan on every pull request.

## Code style

- Keep Dockerfiles minimal - this repo holds base toolchain layers (browsers, interpreters, OS deps), not app code or secrets.
- Pin tool versions explicitly; don't rely on `latest` tags inside a Dockerfile.
- New images need no changes to the publish workflow - it discovers image directories automatically.

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).
