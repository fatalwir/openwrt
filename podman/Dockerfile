FROM docker.io/openwrt/sdk:x86-64

# Set working directory
WORKDIR /home/buildbot
# Switch to buildbot user
USER buildbot

# Clone OpenWrt with hapcan branch
RUN git clone -b hapcan https://github.com/fatalwir/openwrt.git

WORKDIR /home/buildbot/openwrt

RUN ./scripts/feeds update -a && \
    ./scripts/feeds install -a
