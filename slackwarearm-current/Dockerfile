FROM alpine:3.15 as builder

# Use a simple alpine image to download and verify the pieces we need to create
# the base image for slackware arm.

# hadolint ignore=DL3018
RUN apk add --no-cache wget gpg gpg-agent binutils bash

COPY slackwarearm-current/GPG-KEY /tmp/GPG-KEY
RUN gpg --import /tmp/GPG-KEY && rm /tmp/GPG-KEY

SHELL ["/bin/ash", "-eo", "pipefail", "-c"]

WORKDIR /tmp/miniroot

# download, verify and unpack the slackwaream miniroot
RUN \
    wget --quiet https://slackware.uk/slackwarearm/slackwarearm-devtools/minirootfs/roots/slackarm-current-miniroot_details.txt && \
    wget --quiet https://slackware.uk/slackwarearm/slackwarearm-devtools/minirootfs/roots/slackarm-current-miniroot_details.txt.asc && \
    gpg --verify slackarm-current-miniroot_details.txt.asc slackarm-current-miniroot_details.txt && \
    tarball="$(sed -n '/slackarm-current-miniroot_/p' slackarm-current-miniroot_details.txt | awk '{ print $2 }')" && \
    wget --quiet "https://slackware.uk/slackwarearm/slackwarearm-devtools/minirootfs/roots/$tarball" && \
    sed -n '/miniroot_/p' slackarm-current-miniroot_details.txt | sed 's/^[[:space:]]*//' | sha1sum -c - && \
    rm slackarm-current-miniroot_details.txt* && \
    tar -xf "$tarball" && rm "$tarball"

# fetch some packages required to run slackpkg
WORKDIR /tmp/pkgs

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN \
    wget --quiet https://slackware.uk/slackwarearm/slackwarearm-current/slackware/ap/diffutils-3.8-arm-1.txz && \
    wget --quiet https://slackware.uk/slackwarearm/slackwarearm-current/slackware/ap/diffutils-3.8-arm-1.txz.asc && \
    gpg --verify diffutils-3.8-arm-1.txz.asc diffutils-3.8-arm-1.txz && \
    rm diffutils-3.8-arm-1.txz.asc

FROM scratch

COPY --from=builder /tmp/miniroot/. /
COPY --from=builder /tmp/pkgs/*.txz /tmp/

COPY slackwarearm-current/GPG-KEY /tmp/

RUN \
    upgradepkg --install-new /tmp/*.txz && \
    gpg --import /tmp/GPG-KEY && \
    rm /tmp/*

RUN \
    sed -i '/slackwarearm-current/s/^#\(http:\/\/slackware.uk\)/\1/' /etc/slackpkg/mirrors && \
    sed -i 's/DIALOG=on/DIALOG=off/' /etc/slackpkg/slackpkg.conf && \
    sed -i 's/POSTINST=on/POSTINST=off/' /etc/slackpkg/slackpkg.conf && \
    sed -i 's/SPINNING=on/SPINNING=off/' /etc/slackpkg/slackpkg.conf
