# playwright

Node + Playwright image for e2e/a11y smoke test CI jobs (`npm run test:e2e` style scripts
using `@playwright/test` and `@axe-core/playwright`). Based on the official
`mcr.microsoft.com/playwright` image, which already bundles Chromium/Firefox/WebKit and all
required OS-level dependencies, so `npx playwright install --with-deps` is not needed.

## Version pinning

`VERSION` holds this image's own semver, independent of the bundled Playwright version. The
`Dockerfile`'s `PLAYWRIGHT_VERSION` build arg pins the actual `@playwright/test` version this
image bundles - consuming repos must use a matching `@playwright/test` version in
`package.json`, since the browser binaries are version-pinned and a mismatch between the
image and the npm package will error at runtime. Check the image's labels or this README for
the bundled Playwright version; don't assume the image tag matches it.

Bumping Playwright means updating the `Dockerfile`'s `PLAYWRIGHT_VERSION` default and this
README, then cutting a new `VERSION` via `publish.yml`'s `workflow_dispatch` `bump` input (or
by hand) so the image tag changes too.

## Usage

```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/aglflorida/ci-playwright:0.1.0
    steps:
      - uses: actions/checkout@v7
      - run: npm ci
      - run: npm run test:e2e
```
