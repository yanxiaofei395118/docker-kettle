FROM ubuntu:16.04
MAINTAINER Jack Hu <hello@jackhu.top>

ENV DEBIAN_FRONTEND noninteractive
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV TZ "UTC"
ENV VNC_PW "vncpassword"

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