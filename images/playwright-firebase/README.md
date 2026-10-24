# playwright-firebase

`ci-playwright` plus a JRE and `firebase-tools`, for e2e jobs that need to run Playwright
against local Firebase emulators (Auth/Firestore/Functions/Storage) instead of a hosted
Firebase project. Firebase emulators require a JVM, which the base Playwright image doesn't
carry.

## Version pinning

`VERSION` holds this image's own semver - it's independent of the Playwright, JRE, and
firebase-tools versions it bundles, each of which is pinned separately:

- Playwright: pinned transitively. The `Dockerfile`'s `CI_PLAYWRIGHT_VERSION` build arg
  selects which `ci-playwright` tag to build `FROM` - `ci-playwright`'s own `VERSION`, not a
  Playwright version. `publish.yml` passes it explicitly, read live from
  `images/playwright/VERSION`, so there's nothing to keep in sync by hand; the Dockerfile's
  own default (`latest`) only applies to an ad-hoc local `docker build`. Check
  `ci-playwright`'s own Dockerfile/`VERSION` for which `@playwright/test` version a given tag
  bundles; consuming repos must use a matching version in `package.json`, since a mismatch
  between the browser binaries and the npm package will error at runtime.
- `firebase-tools`: this directory's `package.json` (dependabot's `npm` ecosystem tracks it
  there).
- JRE: the `openjdk-21-jre-headless` apt pin in the `Dockerfile`, bumped by hand - dependabot
  has no apt ecosystem.

None of those tool versions are reflected in this image's own tag. Check this README or the
Dockerfile for what a given tag actually bundles. Bump `VERSION` by hand whenever any of them
change.

## Usage

```yaml
jobs:
  e2e:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/aglflorida/ci-playwright-firebase:0.1.0
    steps:
      - uses: actions/checkout@v7
      - run: npm ci
      - run: firebase emulators:exec --project demo-example "npm run test:e2e"
```
