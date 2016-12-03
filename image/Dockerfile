FROM ubuntu:xenial

# Note: phpldapadmin must be installed with a separate invocation of apt-get install;
# otherwise there is an error message about a missing apache files.
RUN apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y install \
      nginx \
      php7.0 php7.0-ldap php7.0-xml \
      supervisor && \
    apt-get -y install phpldapadmin && \
    \
    apt-get -y install curl && \
    \
    curl -L -o /usr/local/bin/confd https://github.com/kelseyhightower/confd/releases/download/v0.11.0/confd-0.11.0-linux-amd64 && \
    chmod +x /usr/local/bin/confd && \
    \
    apt-get -y purge curl && \
    apt-get -y autoremove && \
    \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Verify checksums of downloaded files.
COPY sha256sums.txt /container/
RUN sha256sum -c /container/sha256sums.txt

# Copy configuration files.
COPY nginx.conf /etc/nginx/sites-enabled/default
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY confd/*.toml /etc/confd/conf.d/
COPY confd/*.tmpl /etc/confd/templates/

# Forward nginx request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80

# Environment properties.
# These must be overwritten with appropriate values.
ENV LDAP_URL ldap://127.0.0.1:389
ENV LDAP_BASE dc=example,dc=com
ENV LDAP_ADMIN cn=admin,dc=example,dc=com

# Can be set to the path where a TLS certificates file is bind-mounted.
# ENV LDAP_CERTS /path/to/certs

CMD confd -onetime -backend env && exec /usr/bin/supervisord
