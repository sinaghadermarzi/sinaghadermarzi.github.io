# Reproducible toolchain for local development.
#
# Versions and checksums come from versions.env, which CI reads too, so the
# container and the deploy build always use the identical Hugo binary.

FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

COPY versions.env /tmp/versions.env

RUN set -eux; \
    . /tmp/versions.env; \
    arch="$(dpkg --print-architecture)"; \
    case "$arch" in \
    amd64) hugo_sha="$HUGO_SHA256_AMD64"; go_sha="$GO_SHA256_AMD64" ;; \
    arm64) hugo_sha="$HUGO_SHA256_ARM64"; go_sha="$GO_SHA256_ARM64" ;; \
    *) echo "unsupported architecture: $arch" >&2; exit 1 ;; \
    esac; \
    \
    curl -fsSL -o /tmp/hugo.tar.gz \
    "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-${arch}.tar.gz"; \
    echo "${hugo_sha}  /tmp/hugo.tar.gz" | sha256sum -c -; \
    tar -xzf /tmp/hugo.tar.gz -C /usr/local/bin hugo; \
    \
    curl -fsSL -o /tmp/go.tar.gz \
    "https://go.dev/dl/go${GO_VERSION}.linux-${arch}.tar.gz"; \
    echo "${go_sha}  /tmp/go.tar.gz" | sha256sum -c -; \
    tar -xzf /tmp/go.tar.gz -C /usr/local; \
    \
    rm -f /tmp/hugo.tar.gz /tmp/go.tar.gz /tmp/versions.env

ENV PATH="/usr/local/go/bin:${PATH}"
