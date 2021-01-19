FROM registry.redhat.io/ubi8/s2i-base

ENV BOOTSTRAP=false
ENV USE_DIGESTS=false

USER root

# COPY ./build/dockerfiles/content_set*.repo /etc/yum.repos.d/
COPY ./build/dockerfiles/rhel.install.sh /tmp
RUN /tmp/rhel.install.sh && rm -f /tmp/rhel.install.sh

COPY ./build/scripts ./arbitrary-users-patch/base_images /build/

ENV PATCHED_IMAGES_REG="quay.io"
ENV PATCHED_IMAGES_ORG="eclipse"
ENV PATCHED_IMAGES_TAG="nightly"

COPY ./build/s2i-build/.s2i/bin/ $STI_SCRIPTS_PATH/

RUN chmod -R 755 $STI_SCRIPTS_PATH/ && \
    chown -R 1001:0 ${APP_ROOT} && chmod -R ug+rwx ${APP_ROOT} && \
    rpm-file-permissions

USER 1001