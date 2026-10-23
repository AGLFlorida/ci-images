# ci-images

[![CI](https://github.com/AGLFlorida/ci-images/actions/workflows/ci.yml/badge.svg)](https://github.com/AGLFlorida/ci-images/actions/workflows/ci.yml)

Shared CI container images for AGLFlorida repos, published to GHCR.

## Layout

Each image lives in its own directory under `images/`:

```
images/
  <image-name>/
    Dockerfile
    VERSION
    README.md
```

- `Dockerfile` builds the image. Keep it minimal - this repo holds base toolchain layers
  (browsers, interpreters, OS deps), not app code or secrets.
- `VERSION` holds the semantic tag the publish workflow pushes alongside `latest`
  (e.g. the upstream tool version the image bundles). Bump it whenever the Dockerfile's
  pinned version changes.
- `README.md` documents what the image bundles and how consuming repos should reference it.

Adding a new image means adding a new directory here - no changes to the publish workflow
are needed; it discovers image directories automatically.

## Publishing

`.github/workflows/publish.yml` builds and pushes every image under `images/` on every push
to `main` that touches `images/**` (the trigger's `paths` filter gates whether the workflow
runs at all; it then rebuilds all images, relying on the GitHub Actions layer cache to keep
unchanged images cheap). Pull requests build (but don't push) as a validation check.
`workflow_dispatch` rebuilds everything on demand.

Images are published to `ghcr.io/aglflorida/ci-<image-name>`, tagged with both the
contents of that image's `VERSION` file and `latest`.

## Consuming from another repo

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

Pin to the `VERSION`-file tag, not `latest`, so upgrades are deliberate.
