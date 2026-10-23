ARG PLAYWRIGHT_VERSION=1.64.1

FROM mcr.microsoft.com/playwright:v${PLAYWRIGHT_VERSION}-noble

LABEL org.opencontainers.image.source="https://github.com/AGLFlorida/ci-images"
LABEL org.opencontainers.image.description="Node + Playwright (Chromium/Firefox/WebKit) with OS deps preinstalled for CI e2e/a11y test jobs"
LABEL org.opencontainers.image.licenses="MIT"

# Base image's bundled npm carries fixable CVEs in its own transitive deps
# (brace-expansion, undici); refreshing npm pulls patched versions of both.
RUN npm install -g npm@latest

WORKDIR /workspace
