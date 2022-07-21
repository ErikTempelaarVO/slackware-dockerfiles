ARG base_image=aclemons/slackware:14.2-x84_64-full
# hadolint ignore=DL3006
FROM $base_image

COPY packages/slackpkg+-1.7.0-noarch-13mt.txz /
RUN bash upgradepkg --install-new shadow-4.2.1-x86_64-1.txz && bash /usr/doc/slackpkg+-1.7.0/setupmultilib.sh

COPY scripts/docker-entrypoint.sh /usr/local/bin/
COPY scripts/32dev.sh /etc/profile.d/

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
