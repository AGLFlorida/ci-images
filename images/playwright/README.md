# playwright

Node + Playwright image for e2e/a11y smoke test CI jobs (`npm run test:e2e` style scripts
using `@playwright/test` and `@axe-core/playwright`). Based on the official
`mcr.microsoft.com/playwright` image, which already bundles Chromium/Firefox/WebKit and all
required OS-level dependencies, so `npx playwright install --with-deps` is not needed.

## Version pinning

The `VERSION` file holds the `@playwright/test` version this image bundles. Consuming repos
must use a matching `@playwright/test` version in `package.json` - the browser binaries are
version-pinned and a mismatch between the image and the npm package will error at runtime.

When bumping, update both `VERSION` and the `Dockerfile`'s `PLAYWRIGHT_VERSION` default, then
let the publish workflow build and push the new tag.

## Usage

```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/aglflorida/ci-playwright:1.64.1
    steps:
      - uses: actions/checkout@v7
      - run: npm ci
      - run: npm run test:e2e
```
