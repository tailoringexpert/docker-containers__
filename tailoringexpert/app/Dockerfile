FROM bitnami/java:17

ARG USERNAME=tailoringexpert
ARG USER_UID=1001
ARG USER_GID=$USER_UID
ARG ARG_SSL_KEYSTORE_ALIAS=tailoringexpert
ARG ARG_SSL_KEYSTORE_PASSWORD=changeMe!

ENV APP_VERSION=0.0.7
ENV SERVER_PORT=8443
ENV SERVER_HTTP_PORT=8080
ENV TEMPLATE_HOME=/app/resources/templates
ENV TENANT_CONFIG_HOME=/app/db
ENV JASYPT_PASSWORD=DasIstDasHausVomNikolaus
ENV DEFAULT_DATASOURCE_DRIVER_CLASS=org.mariadb.jdbc.Driver
ENV DEFAULT_DATASOURCE_URL=jdbc:mariadb://tailoringexpert-db:3306/tailoringexpert
ENV DEFAULT_DATASOURCE_USERNAME=tailoringexpert
ENV DEFAULT_DATASOURCE_PASSWORD=ENC(jh7NqelkJ/HSqEg8B5BbbTz4KiJl7V9n)
ENV DEFAULT_DATASOURCE_DATABASE_PLATFORM=org.hibernate.dialect.MariaDBDialect
ENV DEFAULT_DATASOURCE_DATABASE=MYSQL

ENV SSL_ENABLED=false
ENV SSL_PROTOCOL=TLS
ENV SSL_ENABLED_PROTOCOLS=TLSv1.2
ENV SSL_KEYSTORE=/app/lib/tailoringexpert.p12
ENV SSL_KEYSTORE_TYPE=PKCS12
ENV SSL_KEYSTORE_PASSWORD=$ARG_SSL_KEYSTORE_PASSWORD
ENV SSL_KEYSTORE_ALIAS=$ARG_SSL_KEYSTORE_ALIAS


USER 0
RUN install_packages sudo unzip python3 vim runit

RUN addgroup --system --gid $USER_GID $USERNAME &&\
    adduser --system --no-create-home --disabled-password --uid $USER_UID $USERNAME 

ADD rootfs/app/tailoringexpert-distribution*docker.tar.gz  /app
RUN keytool -genkeypair -alias $SSL_KEYSTORE_ALIAS -dname CN=tailoringexpert,OU=tailoringexpert.eu,C=de -keyalg RSA -keysize 2048 -storetype PKCS12 -storepass $SSL_KEYSTORE_PASSWORD -keystore /app/lib/tailoringexpert.p12 -validity 3650 
RUN chmod 755 /app/*.sh  && \
    mkdir -p /app/log /app/db /app/resources/templates /app/resources/catalog && \
	chown -R tailoringexpert:tailoringexpert /app
WORKDIR /app

    
# https://github.com/wk8/docker-runit
RUN mkdir /etc/service/tailoringexpert && \
    /bin/bash -c "echo -e '"'#!/bin/sh -e\nexec 2>&1\nexec chpst -u tailoringexpert /app/start.sh\n'"' > /etc/service/tailoringexpert/run" && \
    chmod +x /etc/service/tailoringexpert/run && \ 	
    ln -s /usr/bin/sv /etc/init.d/tailoringexpert

EXPOSE $SERVER_PORT

CMD ["runsvdir", "/etc/service"]