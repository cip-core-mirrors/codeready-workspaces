FROM registry.redhat.io/rhel8/httpd-24

USER root

RUN chmod +x /usr/share/container-scripts/httpd/pre-init/40-ssl-certs.sh

RUN yum update -y systemd && yum clean all && rm -rf /var/cache/yum && \
    echo "Installed Packages" && rpm -qa | sort -V && echo "End Of Installed Packages"

RUN sed -i /etc/httpd/conf/httpd.conf \
    -e "s,Listen 80,Listen 8080," \
    -e "s,logs/error_log,/dev/stderr," \
    -e "s,logs/access_log,/dev/stdout," \
    -e "s,AllowOverride None,AllowOverride All," && \
    chmod a+rwX /etc/httpd/conf /run/httpd /etc/httpd/logs/

STOPSIGNAL SIGWINCH

WORKDIR /var/www/html

COPY ./build/dockerfiles/rhel.entrypoint.sh ./build/dockerfiles/entrypoint.sh /usr/local/bin/
RUN chmod g+rwX /usr/local/bin/entrypoint.sh /usr/local/bin/rhel.entrypoint.sh

COPY ./build/s2i-run/.s2i/bin/ $STI_SCRIPTS_PATH/ 

RUN chmod -R 755 $STI_SCRIPTS_PATH/ && \
    rpm-file-permissions

USER 1001