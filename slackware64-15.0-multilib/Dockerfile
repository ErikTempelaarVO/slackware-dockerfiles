ARG base_image=aclemons/slackware:14.2-x84_64-full
# hadolint ignore=DL3006
FROM $base_image

RUN bash /usr/doc/slackpkg+-1.8.0/setupmultilib.sh

COPY scripts/docker-entrypoint.sh /usr/local/bin/
COPY scripts/32dev.sh /etc/profile.d/

ENV PATH="/usr/bin/32:/usr/lib/qt/bin:$PATH"
ENV PATH="/opt/build-wrapper-linux-x86:${PATH}"
ENV PATH="/opt/sonar-scanner/bin:${PATH}"
ENV PATH="/opt/sonar-scanner/jre/bin:${PATH}"

ENV CC="gcc"
ENV CXX="g++"

ENV LD_LIBRARY_PATH="/usr/local/lib:/lib:/usr/lib"
ENV PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:/usr/lib/pkgconfig"

ENV PS1='\u@\h (32bit):\w\$ '

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]