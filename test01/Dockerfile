FROM jackhu/docker-kettle:6.1

MAINTAINER Jack Hu <hello@jackhu.top>

ENV VNC_PW "vncpassword"

# config timezone 安装桌面和vnc服务,删除不必要的软件和Apt缓存包列表
RUN apt-get update && \
    apt-get install -y supervisor \
    xfce4 xfce4-goodies \
    x11vnc xvfb