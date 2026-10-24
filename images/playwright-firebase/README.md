# playwright-firebase

`ci-playwright` plus a JRE and `firebase-tools`, for e2e jobs that need to run Playwright
against local Firebase emulators (Auth/Firestore/Functions/Storage) instead of a hosted
Firebase project. Firebase emulators require a JVM, which the base Playwright image doesn't
carry.

## Version pinning

The `VERSION` file holds the `@playwright/test` version this image bundles, same as
`ci-playwright`. Consuming repos must use a matching `@playwright/test` version in
`package.json`.

The `firebase-tools` version is pinned separately in this directory's `package.json`
(dependabot's `npm` ecosystem tracks it there). It isn't tied to the Playwright version.

When bumping Playwright, update both `VERSION` and the `Dockerfile`'s `PLAYWRIGHT_VERSION`
default, then let the publish workflow build and push the new tag.

## Usage

```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/aglflorida/ci-playwright-firebase:1.64.1
    steps:
      - uses: actions/checkout@v7
      - run: npm ci
      - run: firebase emulators:exec --project demo-example "npm run test:e2e"
```
