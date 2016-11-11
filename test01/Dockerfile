FROM ubuntu:16.04

MAINTAINER Jack Hu <hello@jackhu.top>

ENV DEBIAN_FRONTEND noninteractive
ENV VNC_PW "vncpassword"
#RUN locale-gen en_US.UTF-8
#ENV LANG en_US.UTF-8
#ENV LC_ALL en_US.UTF-8

# config timezone 安装桌面和vnc服务,删除不必要的软件和Apt缓存包列表
RUN apt-get update && \
    apt-get install -y supervisor xfce4