# Basis image: ROS 2 Humble met desktop full
ARG BASE_IMAGE=humble-desktop-full

FROM osrf/ros:$BASE_IMAGE

MAINTAINER Gerard Harkema <ga.harkema@avans.nl>

ENV DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-c"]

# install depending packages

# install depending packages
RUN apt-get update && apt-get upgrade -y
RUN    apt-get install -y bash-completion \
    less \
    wget \
    language-pack-en \
#   code \
#   vim-tiny \
    nano \
    iputils-ping \
    net-tools \
    openssh-client \ 
    git \
#   openjdk-8-jdk-headless \
#   nodejs \
    sudo \
#   imagemagick \
#   byzanz \
    python-dev \
    dos2unix\
    supervisor \
#   libsecret-1-dev \
    python-pip
    
#RUN npm install -g yarn


#RUN apt-get update && apt-get upgrade -y && \
#    apt-get install -y bash-completion \
#    less \
#    language-pack-en \
#    nano \
#    dos2unix\
#    iputils-ping \
#    net-tools 
    #\
    #pip




# basic python packages
#RUN apt-get update && pip install --upgrade --ignore-installed --no-cache-dir \
#    pyassimp pylint==1.9.4 \
#    autopep8 \
#    python-language-server[all] \
#    notebook~=5.7 \
#    Pygments \
#    matplotlib \
#    ipywidgets \
#    jupyter_contrib_nbextensions \
#    nbimporter \
#    supervisor \
#    supervisor_twiddler \
#    argcomplete 


RUN apt-get install -y \
    python-catkin-tools \
    libignition-math4 \
    libsdformat6 \
    libsdl-image1.2-dev \
    libnlopt-dev \
	mesa-utils \
	libgl1-mesa-glx 

RUN apt-get install -y \
    ros-$ROS_DISTRO-moveit \
    joint-state-publisher \
    ros-$ROS_DISTRO-joint-state-publisher-gui \
    ros-$ROS_DISTRO-rqt-joint-trajectory-controller\
    ros-$ROS_DISTRO-ros-control \
    ros-$ROS_DISTRO-ros-controllers

# add non-root user
RUN useradd -m developer && \
    echo developer ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer

# install depending packages (install moveit! algorithms on the workspace side, since moveit-commander loads it from the workspace)


# Update en installeer vereiste pakketten
#RUN apt-get install -y \
#    git \
#    curl \
#    wget \
#    lsb-release \
#    python3-pip \
#    software-properties-common \
#    supervisor \
#    dbus-x11 

# Configuratie voor VNC en supervisor
RUN mkdir -p /etc/supervisor/conf.d
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
#RUN dos2unix /etc/supervisor/conf.d/supervisord.conf
#COPY code-server.conf /etc/supervisor/conf.d/code-server.conf

COPY entrypoint.sh /entrypoint.sh
RUN dos2unix /entrypoint.sh

#COPY --from=xsdcache /opt/xsd /opt/xsd

#RUN apt-get install -y \
#    ros-$ROS_DISTRO-moveit \
#    ros-$ROS_DISTRO-moveit-ros-visualization \
#    ros-$ROS_DISTRO-controller-manager-msgs \
#    ros-$ROS_DISTRO-hardware-interface \
#    ros-$ROS_DISTRO-joint-state-broadcaster \
#    ros-$ROS_DISTRO-joint-trajectory-controller \
#    ros-$ROS_DISTRO-joint-state-publisher \
#    ros-$ROS_DISTRO-control-toolbox \
#    ros-$ROS_DISTRO-controller-manager \
#    ros-$ROS_DISTRO-desktop \
#    ros-$ROS_DISTRO-moveit-servo 
#Add own ROS Packeges here

#RUN curl -sSL http://get.gazebosim.org | sh

#RUN apt-get install -y \
#    ros-$ROS_DISTRO-gazebo-msgs \
#    ros-$ROS_DISTRO-gazebo-ros2-control \
#    ros-$ROS_DISTRO-gazebo-plugins \
#    ros-$ROS_DISTRO-gazebo-ros 

# Uncoment for using UR Robots
#RUN apt-get install -y ros-$ROS_DISTRO-ur

RUN rm -rf /var/lib/apt/lists/*

#RUN if [ $(lsb_release -cs) = "focal" ]; then \
#        apt-get update; \
#        apt-get install -y python-is-python3 python3-catkin-tools python3-colcon-common-extensions; \
#        apt-get clean; \
#        curl -kL https://bootstrap.pypa.io/get-pip.py | python; \
#    else \
#        curl -kL https://bootstrap.pypa.io/pip/2.7/get-pip.py | python; \
#    fi && \
#    pip install --upgrade --ignore-installed --no-cache-dir pyassimp pylint==1.9.4 autopep8 python-language-server[all] notebook~=5.7 Pygments matplotlib ipywidgets jupyter_contrib_nbextensions nbimporter supervisor supervisor_twiddler argcomplete




RUN echo $ROS_DISTRO

USER developer
WORKDIR /home/developer

ENV HOME=/home/developer
ENV SHELL=/bin/bash

# init rosdep
#RUN rosdep update

# enter ROS world
RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc

RUN mkdir -p /home/developer/installers
COPY installers /home/developer/installers
RUN sudo chmod -R 777 /home/developer/installers

# Expose ports for ROS communication
EXPOSE 11311

# Command to run roscore
#CMD ["roscore"]

RUN sudo chmod +x /entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "sudo", "-E", "/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
