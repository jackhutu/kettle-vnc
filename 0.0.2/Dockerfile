FROM openjdk:8

MAINTAINER Jack Hu <hello@jackhu.top>

ENV DEBIAN_FRONTEND noninteractive
ENV TZ "UTC"
ENV VNC_PW "vncpassword"

ENV PDI_RELEASE=6.1 \
    PDI_VERSION=6.1.0.1-196 \
    CARTE_PORT=8181 \
    PENTAHO_JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    PENTAHO_HOME=/root/pentaho

RUN apt-get update && \
    apt-get install -y supervisor \
    xfce4 xfce4-goodies \
    x11vnc xvfb && \
    mkdir ~/.vnc && \
    touch ~/.vnc/passwd

#配置VNC密码
RUN x11vnc -storepasswd $VNC_PW ~/.vnc/passwd

# config timezone
RUN echo $TZ > /etc/timezone && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get upgrade --yes

# Download PDI
RUN /usr/bin/wget \
    --progress=dot:giga \
    http://downloads.sourceforge.net/project/pentaho/Data%20Integration/${PDI_RELEASE}/pdi-ce-${PDI_VERSION}.zip \
    -O /tmp/pdi-ce-${PDI_VERSION}.zip && \
    /usr/bin/unzip -q /tmp/pdi-ce-${PDI_VERSION}.zip -d  $PENTAHO_HOME && \
    rm /tmp/pdi-ce-${PDI_VERSION}.zip

# config timezone
RUN echo $TZ > /etc/timezone && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get upgrade --yes

# 删除不必要的软件和Apt缓存包列表
RUN apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root

ADD startup.sh ./
ADD supervisord.conf ./
RUN chmod +x startup.sh

EXPOSE 5900

ENTRYPOINT ["./startup.sh"]