FROM rogueraptor7/ros-noetic

ENV CERES_VERSION="1.12.0"
ENV CATKIN_WS=/root/catkin_ws
ENV ROS_DISTRO=noetic

    # Install dependencies
RUN apt-get update && apt-get install -y \
    cmake \
    libatlas-base-dev \
    libeigen3-dev \
    libgoogle-glog-dev \
    libsuitesparse-dev \
    python3-catkin-tools \
    libpcl-dev pcl-tools \
    liboctomap-dev octovis \
    ros-${ROS_DISTRO}-cv-bridge \
    ros-${ROS_DISTRO}-image-transport \
    ros-${ROS_DISTRO}-message-filters \
    ros-${ROS_DISTRO}-tf && \

    rm -rf /var/lib/apt/lists/* && \
    # Build and install Ceres
    git clone https://github.com/ceres-solver/ceres-solver.git && \
    cd ceres-solver && \
    git checkout tags/${CERES_VERSION} && \
    mkdir build && cd build && \
    cmake .. && \
    make -j${USE_PROC} install && \
    cd ../.. && \
    rm -rf ./ceres-solver && \
    # Setup catkin workspace
    mkdir -p $CATKIN_WS/src/A-LOAM/
    
# WORKDIR $CATKIN_WS/src

# Copy A-LOAM
COPY ../ $CATKIN_WS/src/A-LOAM/
# use the following line if you only have this dockerfile
# RUN git clone https://github.com/HKUST-Aerial-Robotics/A-LOAM.git

# Build A-LOAM
WORKDIR $CATKIN_WS
ENV TERM xterm
ENV PYTHONIOENCODING UTF-8
RUN catkin config \
      --extend /opt/ros/$ROS_DISTRO \
      --cmake-args \
        -DCMAKE_BUILD_TYPE=Release && \
    catkin build && \
    sed -i '/exec "$@"/i \
            source "/root/catkin_ws/devel/setup.bash"' /ros_entrypoint.sh
