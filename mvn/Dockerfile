FROM bitnami/java:17

ARG MAVEN_VERSION=3.9.5
ARG USER_HOME_DIR="/home"
ARG SHA=4810523ba025104106567d8a15a8aa19db35068c8c8be19e30b219a1d7e83bcab96124bf86dc424b1cd3c5edba25d69ec0b31751c136f88975d15406cab3842b
ARG BASE_URL=https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV GNUPGHOME "$USER_HOME_DIR/.gnupg"

USER 0

RUN install_packages ca-certificates curl git gnupg dirmngr gpg

#RUN mkdir $MAVEN_CONFIG $GNUPGHOME   \
#  && chown 1001:1001 $MAVEN_CONFIG $GNUPGHOME

RUN set -eux; curl -fsSLO --compressed ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && echo "${SHA} *apache-maven-${MAVEN_VERSION}-bin.tar.gz" | sha512sum -c - \
  && curl -fsSLO --compressed ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz.asc \
  && export GNUPGHOME="$(mktemp -d)"; \
  for key in \
  6A814B1F869C2BBEAB7CB7271A2A1C94BDE89688 \
  29BEA2A645F2D6CED7FB12E02B172E3E156466E8 \
  ; do \
  gpg --batch --keyserver hkps://keyserver.ubuntu.com --recv-keys "$key" ; \
  done; \
  gpg --batch --verify apache-maven-${MAVEN_VERSION}-bin.tar.gz.asc apache-maven-${MAVEN_VERSION}-bin.tar.gz
RUN mkdir -p ${MAVEN_HOME} ${MAVEN_HOME}/ref \
  && tar -xzf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C ${MAVEN_HOME} --strip-components=1 \
  && ln -s ${MAVEN_HOME}/bin/mvn /usr/bin/mvn \
  && rm apache-maven-${MAVEN_VERSION}-bin.tar.gz

ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV GNUPGHOME "$USER_HOME_DIR/.gnupg"

RUN mkdir $MAVEN_CONFIG $GNUPGHOME   \
  && chmod 600 $GNUPGHOME \
  && chown 1001:1001 $MAVEN_CONFIG $GNUPGHOME

WORKDIR /home


#COPY rootfs /

USER 1001

#ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD ["mvn"]

