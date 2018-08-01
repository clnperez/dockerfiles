## To run this, bind-mount in whatever project you're building to a user's home dir.
## You'll lose your cache when you exit the container though, b/c bazel sym-links the
## output to ~/.cache/bazel/, which is outside the docker build scope on your system.

FROM debian:stretch

ARG BAZEL_RELEASE=0.15.2

RUN apt-get update && apt-get install -y build-essential openjdk-8-jdk python zip git cmake automake wget libtool m4 sudo vim-tiny curl pkg-config ninja-build && \
    rm -rf /var/cache/apt/lists/*

RUN mkdir bazel
WORKDIR bazel
RUN curl -fSL -o bazel-${BAZEL_RELEASE}-dist.zip https://github.com/bazelbuild/bazel/releases/download/${BAZEL_RELEASE}/bazel-${BAZEL_RELEASE}-dist.zip && \
    unzip -q bazel-$BAZEL_RELEASE-dist.zip && \
    ./compile.sh && \
    rm bazel-$BAZEL_RELEASE-dist.zip && \
    mv output/bazel /usr/local/bin/. && \
    cd / && \
    rm -rf /bazel

ARG GO_VERSION=1.10.3
# update go. this version's way too old and buggy for some architectures
RUN set -eux; \
    \
    arch="$(uname -m)"; \
    case "${arch##*-}" in \
        x86_64 | amd64) ARCH='amd64' ;; \
        ppc64el | ppc64le) ARCH='ppc64le' ;; \
        *) echo "unsupported architecture"; exit 1 ;; \
    esac; \
    wget -nv -O - https://storage.googleapis.com/golang/go${GO_VERSION}.linux-${ARCH}.tar.gz \
    | tar -C /usr/local -xz

ENV PATH=$PATH:/usr/local/go/bin/

RUN bazel version
