#FROM webdevops/apache:ubuntu-16.04
FROM webdevops/base:ubuntu-14.04
MAINTAINER info@webdevops.io

ENV WEB_DOCUMENT_ROOT  /application/code/
ENV WEB_DOCUMENT_INDEX index.php
ENV WEB_ALIAS_DOMAIN   *.vm

# Install apache
RUN /usr/local/bin/apt-install \
        apache2 \
        apache2-mpm-worker \
        libapache2-mod-fastcgi \
	&& rm -f /etc/apache2/sites-enabled/* \
	&& a2enmod actions fastcgi ssl rewrite headers

# Deploy scripts/configurations
COPY conf/ /opt/docker/
RUN bash /opt/docker/bin/control.sh provision.role webdevops-apache \
    && bash /opt/docker/bin/bootstrap.sh

RUN chmod +x /opt/docker/bin/service.d/httpd.sh

#EXPOSE 80
#EXPOSE 443
#
#CMD ["supervisord"]

#MAINTAINER Eric Pfeiffer <computerfr33k@users.noreply.github.com>

ARG PHP_SOCKET=php-fpm_op:9000

ENV WEB_PHP_SOCKET=$PHP_SOCKET

ENV WEB_DOCUMENT_ROOT=/var/www/public

#####################################
# Set timezone:
#####################################
ENV SET_CONTAINER_TIMEZONE=true
ENV CONTAINER_TIMEZONE=Asia/Ho_Chi_Minh

RUN if [ ${SET_CONTAINER_TIMEZONE} = true ]; then \
    echo "${CONTAINER_TIMEZONE}" \
    cp /usr/share/zoneinfo/${CONTAINER_TIMEZONE} /etc/localtime && \
    echo "${CONTAINER_TIMEZONE}" >  /etc/timezone && \
    echo "Container timezone set to: $CONTAINER_TIMEZONE" \
else \
    echo "Container timezone not modified" \
;fi

EXPOSE 80 443

WORKDIR /var/www/public

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
