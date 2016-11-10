FROM jackhu/docker-kettle:6.1

MAINTAINER Jack Hu <hello@jackhu.top>

ENV VNC_PW "vncpassword"

# 安装桌面和vnc服务,删除不必要的软件和Apt缓存包列表
RUN apt-get update && \
    apt-get install -y supervisor \
    xfce4 xfce4-goodies \
    x11vnc xvfb

# config timezone
RUN echo $TZ > /etc/timezone && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get upgrade --yes && \
    apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

#配置VNC密码
RUN mkdir ~/.vnc && \
    touch ~/.vnc/passwd && \
    x11vnc -storepasswd $VNC_PW ~/.vnc/passwd

ADD startup.sh $PENTAHO_HOME/docker-entrypoint.d/
ADD supervisord.conf /root/supervisord.conf
RUN chmod +x $PENTAHO_HOME/docker-entrypoint.d/startup.sh

EXPOSE 5900