# playwright-firebase

`ci-playwright` plus a JRE and `firebase-tools`, for e2e jobs that need to run Playwright
against local Firebase emulators (Auth/Firestore/Functions/Storage) instead of a hosted
Firebase project. Firebase emulators require a JVM, which the base Playwright image doesn't
carry.

## Version pinning

`VERSION` holds this image's own semver - it's independent of the Playwright, JRE, and
firebase-tools versions it bundles, each of which is pinned separately:

- Playwright: the `Dockerfile`'s `PLAYWRIGHT_VERSION` build arg. Consuming repos must use a
  matching `@playwright/test` version in `package.json`, since the browser binaries are
  version-pinned and a mismatch between the image and the npm package will error at runtime.
- `firebase-tools`: this directory's `package.json` (dependabot's `npm` ecosystem tracks it
  there).
- JRE: the `openjdk-21-jre-headless` apt pin in the `Dockerfile`, bumped by hand - dependabot
  has no apt ecosystem.

None of those tool versions are reflected in the image tag. Check this README or the
Dockerfile for what a given tag actually bundles. Bump `VERSION` via `publish.yml`'s
`workflow_dispatch` `bump` input (or by hand) whenever any of them change.

## Usage

```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/aglflorida/ci-playwright-firebase:0.2.0
    steps:
      - uses: actions/checkout@v7
      - run: npm ci
      - run: firebase emulators:exec --project demo-example "npm run test:e2e"
```
