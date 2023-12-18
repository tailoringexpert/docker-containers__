FROM bitnami/mariadb

USER 0

RUN install_packages unzip vim python3 python3-docker python3-pymysql openjdk-17-jre
#RUN install_packages procps less sudo 
RUN mkdir -p /app/liquibase /app/changelogs
ADD rootfs/liquibase*.tar.gz /app/liquibase

USER 1002
