## To run this, bind-mount in whatever project you're building to a user's home dir.
## You'll lose your cache when you exit the container though, b/c bazel sym-links the
## output to ~/.cache/bazel/, which is outside the docker build scope on your system.

FROM debian:stretch

RUN apt-get update && apt-get install -y build-essential openjdk-8-jdk python zip
RUN apt-get install -y curl
RUN mkdir bazel
WORKDIR bazel
ENV BAZEL_RELEASE=0.11.1
RUN curl -fSL -o bazel-${BAZEL_RELEASE}-dist.zip https://github.com/bazelbuild/bazel/releases/download/${BAZEL_RELEASE}/bazel-${BAZEL_RELEASE}-dist.zip
RUN unzip -q bazel-$BAZEL_RELEASE-dist.zip && \
    ./compile.sh
ENV PATH=$PATH:/bazel/output/

# CLEAN UP LATER -- things needed for building envoy
RUN apt-get update && apt-get install -y git golang-go cmake automake wget libtool m4 sudo vim-tiny

RUN bazel version
