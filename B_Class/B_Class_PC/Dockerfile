FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages for ROS 2 and ssh
RUN apt-get update && apt-get install -y \
    apt-utils \
    iputils-ping \
    iproute2 \
    net-tools \
    ethtool \
    openssh-client \
    curl \
    gnupg2 \
    lsb-release \
    build-essential \
    python3-pip \
    locales \
    software-properties-common \
    libgl1-mesa-glx x11-apps \
    && rm -rf /var/lib/apt/lists/*

# Setup ROS 2 locale
RUN locale-gen en_US en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && export LANG=en_US.UTF-8

# Setup ROS 2 source
RUN add-apt-repository universe \
    && curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Install ROS 2 package
RUN apt-get update && apt-get install -y \
    ros-galactic-desktop \
    ros-galactic-ros-base \
    ros-dev-tools \
    && echo 'source /opt/ros/galactic/setup.sh' >> /root/.bashrc \
    && rm -rf /var/lib/apt/lists/*

