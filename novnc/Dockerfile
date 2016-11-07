FROM openjdk:8

MAINTAINER Jack Hu <hello@jackhu.top>

# Set required environment vars
ENV PDI_RELEASE=6.1 \
    PDI_VERSION=6.1.0.1-196 \
    CARTE_PORT=8080 \
    PENTAHO_JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    PENTAHO_HOME=/home/pentaho \
    VNC_PW "vncpassword" \
    DISPLAY :0

#升级仓库，安装xvfb x11vnc fluxbox
RUN apt-get update -y &&  apt-get -y install xvfb x11vnc fluxbox xdotool  git git-core ca-certificates && \
    mkdir ~/.vnc && \
    touch ~/.vnc/passwd

#配置VNC密码
RUN x11vnc -storepasswd $VNC_PW ~/.vnc/passwd
# Clone noVNC, and set vnc_auto as homepage
RUN git clone --recursive https://github.com/kanaka/noVNC.git /opt/novnc && \
        git clone --recursive https://github.com/kanaka/websockify.git /opt/novnc/utils/websockify && \
        ln -s /opt/novnc/vnc.html /opt/novnc/index.html

# 升级到最新版本
RUN apt-get upgrade --yes

# 删除不必要的软件和Apt缓存包列表
RUN apt-get autoclean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

# Docker's supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Create user
RUN mkdir ${PENTAHO_HOME} && \
    groupadd -r pentaho && \
    useradd -s /bin/bash -d ${PENTAHO_HOME} -r -g pentaho pentaho && \
    chown pentaho:pentaho ${PENTAHO_HOME}

RUN chown -R pentaho:pentaho $PENTAHO_HOME

# Switch to the pentaho user
USER pentaho

# Download PDI
RUN wget --progress=dot:giga \
    http://downloads.sourceforge.net/project/pentaho/Data%20Integration/${PDI_RELEASE}/pdi-ce-${PDI_VERSION}.zip \
    -O /tmp/pdi-ce-${PDI_VERSION}.zip && \
    unzip -q /tmp/pdi-ce-${PDI_VERSION}.zip -d  $PENTAHO_HOME && \
    rm /tmp/pdi-ce-${PDI_VERSION}.zip

# We can only add KETTLE_HOME to the PATH variable now
# as the path gets eveluated - so it must already exist
ENV KETTLE_HOME=$PENTAHO_HOME/data-integration \
    PATH=$KETTLE_HOME:$PATH

# Expose Carte Server # Expose VNC port，5900为VNC客户端登录的端口，8787为novnc
EXPOSE ${CARTE_PORT} 5900 8787

# As we cannot use env variable with the entrypoint and cmd instructions
# we set the working directory here to a convenient location
# We set it to KETTLE_HOME so we can start carte easily
WORKDIR $KETTLE_HOME

# Run Service
CMD ["/usr/bin/supervisord"]