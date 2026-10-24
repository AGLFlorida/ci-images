ARG CI_PLAYWRIGHT_VERSION=0.1.0

FROM ghcr.io/aglflorida/ci-playwright:${CI_PLAYWRIGHT_VERSION}

LABEL org.opencontainers.image.source="https://github.com/AGLFlorida/ci-images"
LABEL org.opencontainers.image.description="ci-playwright plus a JRE and firebase-tools, for e2e jobs that run against Firebase emulators"
LABEL org.opencontainers.image.licenses="MIT"

WORKDIR /workspace

# package.json pins the firebase-tools version so dependabot's npm
# ecosystem can bump it; it is not otherwise used by this image.
COPY package.json .

# Firebase emulators (Firestore, Auth, etc.) run on the JVM.
RUN apt-get update \
    && apt-get install -y --no-install-recommends openjdk-21-jre-headless=21.0.12.1+1-1~24.04.4 \
    && rm -rf /var/lib/apt/lists/* \
    && npm install -g "firebase-tools@$(node -p "require('./package.json').dependencies['firebase-tools']")"
