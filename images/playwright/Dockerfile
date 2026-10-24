ARG PLAYWRIGHT_VERSION=1.64.1

FROM mcr.microsoft.com/playwright:v${PLAYWRIGHT_VERSION}-noble

LABEL org.opencontainers.image.source="https://github.com/AGLFlorida/ci-images"
LABEL org.opencontainers.image.description="Node + Playwright (Chromium/Firefox/WebKit) with OS deps preinstalled for CI e2e/a11y test jobs"
LABEL org.opencontainers.image.licenses="MIT"

# Base image's bundled npm carries fixable CVEs in its own transitive deps
# (brace-expansion, undici); refreshing npm pulls patched versions of both.
# npm still nests its own undici copy independent of the global version, so
# for CVEs disclosed after npm's own last release, drop a patched undici
# into that nested copy directly (a plain `npm install --prefix` there
# treats npm's internals as a project and reinstalls its whole tree).
RUN npm install -g npm@latest \
    && UNDICI_DIR="$(npm root -g)/npm/node_modules/undici" \
    && TMP="$(mktemp -d)" \
    && npm pack undici@6.29.1 --pack-destination "$TMP" --silent \
    && tar -xzf "$TMP"/undici-*.tgz -C "$TMP" \
    && rm -rf "$UNDICI_DIR" \
    && mv "$TMP/package" "$UNDICI_DIR" \
    && rm -rf "$TMP"

WORKDIR /workspace
