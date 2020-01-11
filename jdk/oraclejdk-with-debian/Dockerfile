FROM debian:10-slim

LABEL maintainer="276516848@qq.com"

ARG TZ="Asia/Shanghai"
ARG JAVA_VERSION=8u231
ARG JCE_POLICY_VERSION=8
ARG TINI_VERSION=v0.18.0
ARG TINI_DOWNLOAD_URL=https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-amd64
ARG JDK_DOWNLOAD_URL
ARG JCE_DOWNLOAD_URL

ENV LC_ALL=zh_CN.UTF-8 \
    LANG=zh_CN.UTF-8 \
    LANGUAGE=zh_CN:zh \
    JAVA_HOME=/usr/local/jdk

ENV PATH $JAVA_HOME/bin:$PATH

RUN set -ex \
    && echo deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free > /etc/apt/sources.list \
    && echo deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free >> /etc/apt/sources.list \
    && echo deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free >> /etc/apt/sources.list \
    && echo deb http://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free >> /etc/apt/sources.list \
    && apt update -y && apt upgrade -y \
    && apt install -y locales wget unzip iputils-ping \
    && wget ${TINI_DOWNLOAD_URL} -O /usr/bin/tini \
    && chmod +x /usr/bin/tini \
    && locale-gen --purge zh_CN.UTF-8 \
    && localedef -i zh_CN -c -f UTF-8 zh_CN.UTF-8 \
    && echo 'LANG="zh_CN.UTF-8"' > /etc/default/locale \
    && echo 'LANGUAGE="zh_CN.UTF-8"' >> /etc/default/locale \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo "${TZ}" > /etc/timezone \
    && cd /usr/share/i18n/charmaps \
    && ls|egrep -v 'UTF-8.gz|GB18030.gz|GBK.gz|GB2312.gz|ISO-8859-1.gz'|xargs rm -rf \
    && cd /usr/share/i18n/locales \
    && ls|egrep -v 'zh_CN'|xargs rm -rf \
    && echo 'zh_CN.UTF-8 UTF-8\nzh_CN.GB18030 GB18030\nzh_CN.GBK GBK\nzh_CN GB2312' > /usr/share/i18n/SUPPORTED \
    && mkdir -p ${JAVA_HOME} \
    && wget ${JDK_DOWNLOAD_URL} -O /tmp/jdk.tar.gz \
    && wget ${JCE_DOWNLOAD_URL} -O /tmp/jce.zip \
    && tar -xzf /tmp/jdk.tar.gz --strip-components=1 -C ${JAVA_HOME} \
    && unzip -jo -d ${JAVA_HOME}/jre/lib/security /tmp/jce.zip \ 
    && apt remove -y unzip wget openssl ca-certificates libpcre2-8-0 libpsl5 libssl1.1 publicsuffix \
    && apt autoremove -y \
    && rm -rf   /var/lib/apt/lists/* \
                /var/log/* \
                /var/cache/* \
                /tmp/* \
                ${JAVA_HOME}/*src.zip \
                ${JAVA_HOME}/man \
                ${JAVA_HOME}/*.txt \
                ${JAVA_HOME}/*.html \
                ${JAVA_HOME}/COPYRIGHT \
                ${JAVA_HOME}/LICENSE \
                ${JAVA_HOME}/release \
                ${JAVA_HOME}/bin/jmc* \
                ${JAVA_HOME}/bin/jvisualvm \
                ${JAVA_HOME}/bin/*javafx* \
                ${JAVA_HOME}/bin/appletviewer \
                ${JAVA_HOME}/bin/javapackager \
                ${JAVA_HOME}/bin/jconsole \
                ${JAVA_HOME}/bin/ControlPanel \
                ${JAVA_HOME}/bin/jcontrol \
                ${JAVA_HOME}/lib/missioncontrol \
                ${JAVA_HOME}/lib/visualvm \
                ${JAVA_HOME}/lib/*javafx* \
                ${JAVA_HOME}/lib/jconsole.jar \
                ${JAVA_HOME}/lib/packager.jar \
                ${JAVA_HOME}/jre/*.txt \
                ${JAVA_HOME}/jre/*.html \
                ${JAVA_HOME}/jre/COPYRIGHT \
                ${JAVA_HOME}/jre/LICENSE \
                ${JAVA_HOME}/jre/README \
                ${JAVA_HOME}/jre/plugin \
                ${JAVA_HOME}/jre/bin/ControlPanel \
                ${JAVA_HOME}/jre/bin/jcontrol \
                ${JAVA_HOME}/jre/lib/deploy* \
                ${JAVA_HOME}/jre/lib/desktop \
                ${JAVA_HOME}/jre/lib/*javafx* \
                ${JAVA_HOME}/jre/lib/*jfx* \
                ${JAVA_HOME}/jre/lib/oblique-fonts \
                ${JAVA_HOME}/jre/lib/plugin.jar \
                ${JAVA_HOME}/jre/lib/amd64/libavplugin*.so \
                ${JAVA_HOME}/jre/lib/amd64/libde*.so \
                ${JAVA_HOME}/jre/lib/amd64/libfxplugins.so \
                ${JAVA_HOME}/jre/lib/amd64/libgl*.so \
                ${JAVA_HOME}/jre/lib/amd64/libgstreamer-lite.so \
                ${JAVA_HOME}/jre/lib/amd64/libjavafx*.so \
                ${JAVA_HOME}/jre/lib/amd64/libjfx*.so \
                ${JAVA_HOME}/jre/lib/amd64/libnpjp2.so \
                ${JAVA_HOME}/jre/lib/amd64/libprism_*.so \
                ${JAVA_HOME}/jre/lib/amd64/libfxplugins.so \
                ${JAVA_HOME}/jre/lib/amd64/server/libjsig.so \
                ${JAVA_HOME}/jre/lib/deploy \
                ${JAVA_HOME}/jre/lib/images/icons \
                ${JAVA_HOME}/jre/lib/locale \
                ${JAVA_HOME}/jre/lib/ext/jfxrt.jar \
                ${JAVA_HOME}/jre/lib/ext/jaccess.jar \
    && ln -s ${JAVA_HOME}/jre/lib/amd64/libjsig.so ${JAVA_HOME}/jre/lib/amd64/server/libjsig.so

ENTRYPOINT ["tini","--"]

CMD ["bash"]
