#FROM althack/ros2:humble-dev 
FROM arm64v8/ros:humble-perception

# 非対話型インストール設定
ARG DEBIAN_FRONTEND=noninteractive

# ROSユーザーの作成
RUN useradd -m ros && \
    mkdir -p /home/ros && \
    chown -R ros:ros /home/ros

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    build-essential \
    gdb \
    cmake \
    git \
    vim \
    wget \
    python3-pip \
    python3-libgpiod && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install ROS packages
RUN apt-get update && apt-get install -y \
  ros-humble-urg-node \
  ros-humble-sensor-msgs \
  ros-humble-rviz2 \
  ros-humble-rmw-cyclonedds-cpp \
  ros-humble-geometry-msgs \
  ros-humble-nav-msgs \
  ros-humble-tf2 \
  ros-humble-rclcpp && \
  apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
  pyserial==3.5 \
  spidev==3.6 \
  rpi-lgpio==0.6 \
  gpiozero==2.0.1 \
  transforms3d==0.4.2 \
  matplotlib==3.9.0

ENV RMW_IMPLEMENTATION rmw_cyclonedds_cpp

# entrypoint.sh スクリプトをコンテナにコピー
COPY entrypoint.sh /entrypoint.sh

# スクリプトを実行可能にする
RUN chmod +x /entrypoint.sh

# スクリプトをエントリーポイントとして設定
ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]

# 作業ユーザーを設定
USER ros
WORKDIR /home/ros

# .bashrc の設定
RUN echo "if [ -f /workspaces/icart_ws/install/setup.bash ]; then source /workspaces/icart_ws/install/setup.bash; fi" >> /home/ros/.bashrc

# エントリポイントスクリプトの設定
