# Security Policy

## Supported versions

| Branch | Supported |
|--------|-----------|
| `main` | Yes       |

Only the latest published image tags (built from `main`) are actively supported. There are no versioned releases of this repo itself.

## Reporting a vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Use GitHub's private vulnerability reporting instead:
[Report a vulnerability](https://github.com/AGLFlorida/ci-images/security/advisories/new)

You should receive an acknowledgment within 48 hours. We will work with you to understand the scope and issue a fix as quickly as possible.

## Scope

ci-images builds and publishes shared CI container images to GHCR for other AGLFlorida repos to consume. The primary security surface is:

- **Base image and dependency CVEs** - each image is scanned with Trivy on every pull request; critical/high severity findings fail the build.
- **Build provenance** - images are built and pushed only from `main` via the `publish` workflow, using a scoped `GITHUB_TOKEN`; no long-lived credentials are stored in the repo.
- **Runner hardening** - CI jobs run under `step-security/harden-runner` to monitor and restrict outbound network access during builds.

If you discover a vulnerable dependency, a way to inject into the build process, or a way to push an image under this org's name without going through the `publish` workflow, please report it privately.
