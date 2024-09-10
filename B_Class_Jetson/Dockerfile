FROM nvcr.io/nvidia/l4t-base:35.3.1

ENV DEBIAN_FRONTEND=noninteractive

COPY Camera_Test /app/Camera_Test

WORKDIR /app

# Install necessary packages for ROS 2 and ssh
RUN apt-get update && apt-get install -y \
    apt-utils \
    iputils-ping \
    iproute2 \
    net-tools \
    ethtool \
    openssh-client \
    openssh-server \
    curl \
    gnupg2 \
    lsb-release \
    build-essential \
    locales \
    software-properties-common \
    v4l-utils \
    nano \
    && rm -rf /var/lib/apt/lists/*

# Setup SSH service
RUN mkdir /var/run/sshd \
    && echo 'root:6509' | chpasswd \
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && echo "PermitRootLogin yes" >> /etc/ssh/sshd_config \
    && echo "AllowAgentForwarding yes" >> /etc/ssh/sshd_config \
    && echo "AllowTcpForwarding yes" >> /etc/ssh/sshd_config \
    && echo "X11Forwarding yes" >> /etc/ssh/sshd_config \
    && echo "X11DisplayOffset 10" >> /etc/ssh/sshd_config \
    && echo "X11UseLocalhost no" >> /etc/ssh/sshd_config 
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

# Install Python 3.8
RUN add-apt-repository ppa:deadsnakes/ppa && apt-get update && apt-get install -y \
    python3.8 python3.8-dev python3.8-distutils \
    && curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py \
    && python3.8 get-pip.py \
    && rm get-pip.py

# Set Python 3.8 as the default python
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1

# Install specific versions of OpenCV and PyQt5
RUN pip3 install opencv-python==4.8.1.78 PyQt5==5.14.1

# Expose port 22 for SSH    
EXPOSE 22

# Run SSH service
CMD ["/usr/sbin/sshd", "-D"]