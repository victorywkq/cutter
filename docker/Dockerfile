FROM ubuntu:rolling
LABEL maintainer "pschmied <ps1337@mailbox.org>"

# Dependencies
RUN apt-get update && \
    apt-get -y install \
    cmake \
    curl \
    g++ \
    gcc \
    git-core \
    gosu \
    libqt5svg5-dev \
    make \
    pkg-config \
    python3 \
    python3-dev \
    qt5-default \
    qtbase5-dev \
    qtwebengine5-dev \
    unzip \
    wget

# Get latest cutter release
WORKDIR /opt
RUN git clone --recurse-submodules https://github.com/radareorg/cutter.git && \
    cd cutter && \
    git checkout tags/$(git tag | tail -1)

# Build cutter
WORKDIR /opt/cutter
RUN bash build.sh

# Add r2 user
RUN useradd r2

# Prepare files to mount configurations later on
RUN mkdir /var/sharedFolder && \
    mkdir -p /home/r2/.config/radare2 && \
    touch /home/r2/.radare2rc && \
    chown -R r2:r2 /var/sharedFolder && \
    chown -R r2:r2 /home/r2/

WORKDIR /home/r2
ADD entrypoint.sh /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
