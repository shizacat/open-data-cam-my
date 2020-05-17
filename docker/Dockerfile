FROM nvcr.io/nvidia/l4t-base:r32.4.2

# Install need packages
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        libgstreamer1.0-0 \
        gstreamer1.0-plugins-base \
        gstreamer1.0-plugins-good \
        gstreamer1.0-plugins-bad \
        gstreamer1.0-plugins-ugly \
        gstreamer1.0-libav \
        gstreamer1.0-doc \
        gstreamer1.0-tools \
        libgstreamer1.0-dev \
        libgstreamer-plugins-base1.0-dev && \
    apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/*

# Install OpenCV
# Jetson Nano
ENV OPENCV_VERSION=4.3.0
ENV OPENCV_SOURCE_DIR=/root
ENV ARCH_BIN=5.3
ENV INSTALL_DIR=/usr/local
ENV CMAKE_INSTALL_PREFIX=$INSTALL_DIR
ENV PACKAGE_OPENCV="-D CPACK_BINARY_DEB=ON"
RUN apt-get update && \
    apt-get install -y software-properties-common git && \
    apt-add-repository universe && \
    apt-get update && \
    apt-get install -y \
        build-essential \
        cmake \
        libavcodec-dev \
        libavformat-dev \
        libavutil-dev \
        libeigen3-dev \
        libglew-dev \
        libgtk2.0-dev \
        libgtk-3-dev \
        libjpeg-dev \
        libpng-dev \
        libpostproc-dev \
        libswscale-dev \
        libtbb-dev \
        libtiff5-dev \
        libv4l-dev \
        libxvidcore-dev \
        libx264-dev \
        qt5-default \
        zlib1g-dev \
        pkg-config \
        qt5-default \
        g++-aarch64-linux-gnu && \
    apt-get install -y python3-dev python3-numpy python3-py python3-pytest && \
    apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev && \
    cd $OPENCV_SOURCE_DIR && \
    git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv.git && \
    git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv_contrib.git && \
    cd $OPENCV_SOURCE_DIR/opencv &&  mkdir build && cd build && \
    time cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} \
        -D WITH_CUDA=ON \
        -D CUDA_ARCH_BIN=${ARCH_BIN} \
        -D CUDA_ARCH_PTX="" \
        -D ENABLE_FAST_MATH=ON \
        -D CUDA_FAST_MATH=ON \
        -D WITH_CUBLAS=ON \
        -D WITH_LIBV4L=ON \
        -D WITH_V4L=ON \
        -D WITH_GSTREAMER=ON \
        -D WITH_GSTREAMER_0_10=OFF \
        -D WITH_QT=ON \
        -D WITH_OPENGL=ON \
        -D BUILD_opencv_python2=OFF \
        -D BUILD_opencv_python3=ON \
        -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF \
        -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
        -D WITH_TENGINE=ON \
        $"PACKAGE_OPENCV" \
        ../ && \
    time make -j$(nproc) && \
    make install && \
    ldconfig && \
    apt-get remove cmake build-essential git g++ && \
    apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/*
