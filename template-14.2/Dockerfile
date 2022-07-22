ARG base_image=aclemons/slackware:14.2-x84_64-full
# hadolint ignore=DL3006
FROM $base_image

COPY packages/slackpkg+-1.7.0-noarch-13mt.txz /
COPY packages/shadow-4.2.1-x86_64-1.txz /
COPY scripts/install_template.sh /
COPY templates/ /etc/slackpkg/templates/

ARG post_install=script/empty_post_install.sh
COPY "$post_install" /post_install.sh

# install patched shadow that accepts long groupnames for azure devops compatibility
RUN bash upgradepkg --install-new shadow-4.2.1-x86_64-1.txz && \
    chmod 4755 /bin/su && chmod 4755 /bin/login && \
    bash upgradepkg --install-new slackpkg+-1.7.0-noarch-13mt.txz && \
    chmod -R a+rwx /opt/ 

ARG base_image=aclemons/slackware:14.2-x84_64-full
ARG mirror
ARG template

RUN bash /install_template.sh "$base_image" "$mirror" "$template" && rm /install_template.sh
