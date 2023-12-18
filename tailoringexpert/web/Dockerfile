FROM bitnami/apache

ENV APACHE_HTTP_PORT_NUMBER=8080
ENV APACHE_HTTPS_PORT_NUMBER=8443
ENV APACHE_SERVER_NAME=localhost
ENV APP_SERVER_PROTOCOL=http
ENV APP_SERVER_PORT=8080

ARG APP_TENANT=demo

USER 0

RUN install_packages unzip vim python3
#optional "debug" packages
#RUN install_packages procps iputils-ping netcat curl

RUN sed -i -r "s/#LoadModule proxy_module/LoadModule proxy_module/" /opt/bitnami/apache/conf/httpd.conf
RUN sed -i -r "s/#LoadModule proxy_http_module/LoadModule proxy_http_module/" /opt/bitnami/apache/conf/httpd.conf

ADD rootfs/app/tailoringexpert-web-*docker.tar.gz  /app
RUN sed -i -r "s/\/\/ CONFIGURATIONS_PLACEHOLDER/window.configs = { \"VUE_APP_TENANT\": \"${APP_TENANT}\" }/" /app/index.html
ADD rootfs/vhosts /vhosts

WORKDIR /app

USER 1001
