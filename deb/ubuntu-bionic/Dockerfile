ARG GO_IMAGE
ARG DISTRO=ubuntu
ARG SUITE=bionic
ARG BUILD_IMAGE=${DISTRO}:${SUITE}

FROM ${GO_IMAGE} AS golang

FROM ${BUILD_IMAGE}

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y curl devscripts equivs git

ENV GOPROXY=direct
ENV GO111MODULE=off
ENV GOPATH /go
ENV PATH $PATH:/usr/local/go/bin:$GOPATH/bin

ARG COMMON_FILES
COPY ${COMMON_FILES} /root/build-deb/debian
RUN apt-get update \
 && mk-build-deps -t "apt-get -o Debug::pkgProblemResolver=yes --no-install-recommends -y" -i /root/build-deb/debian/control

COPY sources/ /sources
ARG DISTRO
ARG SUITE
ENV DISTRO=${DISTRO}
ENV SUITE=${SUITE}

COPY --from=golang /usr/local/go /usr/local/go

WORKDIR /root/build-deb
COPY build-deb /root/build-deb/build-deb

ENTRYPOINT ["/root/build-deb/build-deb"]
