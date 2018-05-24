## usage:
## $ docker build qemu-test .
## $ docker run --rm -it qemu:test


FROM debian:buster-slim as qemu
RUN apt-get update && apt-get install --no-install-recommends -y qemu-user-static
##
FROM ppc64le/ubuntu:16.04
COPY --from=qemu /usr/bin/qemu-ppc64le-static /usr/bin/qemu-ppc64le-static
RUN ["/usr/bin/qemu-ppc64le-static", "/bin/echo", "hi"]


