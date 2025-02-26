FROM osrf/ros:humble-desktop

# Add ubuntu user with same UID and GID as your host system, if it doesn't already exist
# Since Ubuntu 24.04, a non-root user is created by default with the name vscode and UID=1000
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if ! id -u $USER_UID >/dev/null 2>&1; then \
        groupadd --gid $USER_GID $USERNAME && \
        useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi
# Add sudo support for the non-root user
RUN apt-get update && \
    apt-get install -y sudo && \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Switch from root to user
USER $USERNAME

# Add user to video group to allow access to webcam
RUN sudo usermod --append --groups video $USERNAME

# Update all packages
RUN sudo apt update && sudo apt upgrade -y

# Install Git
RUN sudo apt update && sudo apt install -y \
    git \
    ros-humble-rosidl-typesupport-c \
    ros-humble-rosidl-default-generators \
    ros-humble-robot-state-publisher \
    ros-humble-builtin-interfaces \
    python3-colcon-common-extensions \
    build-essential \
    && sudo rm -rf /var/lib/apt/lists/*

# Install additional ROS 2 development dependencies
RUN sudo apt update && sudo apt install -y \
    ros-humble-ros2-control \
    ros-humble-ros2-controllers \
    ros-humble-can-msgs \ 
    ros-humble-ros2-socketcan \
    ros-humble-twist-mux \
    ros-humble-rosidl-typesupport-c \
    ros-humble-rosidl-default-generators \
    ros-humble-robot-state-publisher \
    ros-humble-builtin-interfaces \
    python3-colcon-common-extensions \
    build-essential \
    && sudo rm -rf /var/lib/apt/lists/*

# Rosdep update
RUN rosdep update

# Source the ROS setup file
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

################################
## ADD ANY CUSTOM SETUP BELOW ##
################################
