## -*- docker-image-name: "docker-crate" -*-
#
# Crate Dockerfile
# https://github.com/crate/docker-crate

# 根据官方 Dockerfile 修改，支持 crate-ce

FROM centos:7

RUN groupadd crate && useradd -u 1000 -g crate -d /crate crate

RUN curl --retry 8 -o /openjdk.tar.gz https://download.java.net/java/GA/jdk12.0.1/69cfe15208a647278a19ef0990eea691/12/GPL/openjdk-12.0.1_linux-x64_bin.tar.gz \
    && echo "151eb4ec00f82e5e951126f572dc9116104c884d97f91be14ec11e85fc2dd626 */openjdk.tar.gz" | sha256sum -c - \
    && tar -C /opt -zxf /openjdk.tar.gz \
    && rm /openjdk.tar.gz

ENV JAVA_HOME /opt/jdk-12.0.1

# REF: https://github.com/elastic/elasticsearch-docker/issues/171
RUN ln -sf /etc/pki/ca-trust/extracted/java/cacerts /opt/jdk-12.0.1/lib/security/cacerts

# install crate
RUN yum install -y yum-utils https://centos7.iuscommunity.org/ius-release.rpm \
    && yum makecache \
    && yum install -y python36u openssl \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && curl -fSL -O https://raw.githubusercontent.com/msdemt/dockerfiles/master/crate-ce/crate-ce-4.0.10-.tar.gz \
    #&& curl -fSL -O https://cdn.crate.io/downloads/releases/crate-4.0.10.tar.gz.asc \
    #&& export GNUPGHOME="$(mktemp -d)" \
    #&& gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 90C23FC6585BC0717F8FBFC37FAAE51A06F6EAEB \
    #&& gpg --batch --verify crate-4.0.10.tar.gz.asc crate-4.0.10.tar.gz \
    #&& rm -rf "$GNUPGHOME" crate-4.0.10.tar.gz.asc \
    && tar -xf crate-ce-4.0.10-.tar.gz -C /crate --strip-components=1 \
    && rm crate-ce-4.0.10-.tar.gz \
    && ln -sf /usr/bin/python3.6 /usr/bin/python3 \
    && ln -sf /usr/bin/python3.6 /usr/bin/python

#COPY --chown=1000:0 config/crate.yml /crate/config/crate.yml
#COPY --chown=1000:0 config/log4j2.properties /crate/config/log4j2.properties

# install crash
RUN curl -fSL -O https://cdn.crate.io/downloads/releases/crash_standalone_0.24.2 \
    && curl -fSL -O https://cdn.crate.io/downloads/releases/crash_standalone_0.24.2.asc \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 90C23FC6585BC0717F8FBFC37FAAE51A06F6EAEB \
    && gpg --batch --verify crash_standalone_0.24.2.asc crash_standalone_0.24.2 \
    && rm -rf "$GNUPGHOME" crash_standalone_0.24.2.asc \
    && mv crash_standalone_0.24.2 /usr/local/bin/crash \
    && chmod +x /usr/local/bin/crash

ENV PATH /crate/bin:$PATH
# Default heap size for Docker, can be overwritten by args
ENV CRATE_HEAP_SIZE 512M

RUN mkdir -p /data/data /data/log

VOLUME /data

WORKDIR /data

# http: 4200 tcp
# transport: 4300 tcp
# postgres protocol ports: 5432 tcp
EXPOSE 4200 4300 5432

# These COPY commands have been moved before the last one due to the following issues:
# https://github.com/moby/moby/issues/37965#issuecomment-448926448
# https://github.com/moby/moby/issues/38866
COPY --chown=1000:0 crate.yml /crate/config/crate.yml
COPY --chown=1000:0 config/log4j2.properties /crate/config/log4j2.properties

LABEL maintainer="Crate.io <office@crate.io>" \
    org.opencontainers.image.created="2019-12-10T13:14:43.986536" \
    org.opencontainers.image.title="crate" \
    org.opencontainers.image.description="CrateDB is a distributed SQL database handles massive amounts of machine data in real-time." \
    org.opencontainers.image.url="https://crate.io/products/cratedb/" \
    org.opencontainers.image.source="https://github.com/crate/docker-crate" \
    org.opencontainers.image.vendor="Crate.io" \
    org.opencontainers.image.version="4.0.10"

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["crate"]
