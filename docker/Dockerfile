FROM nvidia/cuda:11.1.1-base-ubuntu18.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    wget \
    curl \
    git \
    vim \
    tree \
    less \
    unzip \
    build-essential \
    cmake \
    libeigen3-dev \
    libopencv-dev \
    libgflags-dev \
    libgoogle-glog-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    freeglut3-dev \
    mesa-utils \
    libegl1 \
    libegl1-mesa-dev \
    libgles2-mesa-dev \
    python3.6 \
    python3.6-dev \
    python3-pip && \
    rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.6 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

RUN pip install --upgrade pip && \
    pip install torch==1.10.0+cu111 torchvision==0.11.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html && \
    pip install setuptools tqdm requests matplotlib scipy imgaug pandas

WORKDIR /root
RUN git clone --recurse-submodules --branch verify-dec-2024 --single-branch https://github.com/tsuyoshimishina/ZebraPose.git
WORKDIR /root/ZebraPose/zebrapose
RUN wget --no-verbose https://zebrapose-verify-dec-2024.s3.ap-northeast-1.amazonaws.com/external_assets.tar.gz && \
    tar -xvzf external_assets.tar.gz > /dev/null && \
    rm external_assets.tar.gz
WORKDIR /root/ZebraPose/bop_toolkit
RUN pip install Cython==0.29.24 && \
    pip install -r requirements.txt -e .

WORKDIR /root
RUN git clone --recurse-submodules --branch verify-dec-2024 --single-branch https://github.com/tsuyoshimishina/progressive-x.git
WORKDIR /root/progressive-x/build
RUN cmake .. -DCMAKE_PREFIX_PATH="/usr/lib/x86_64-linux-gnu/cmake/gflags" && \
    make
WORKDIR /root/progressive-x
RUN pip install -e .

RUN echo "export DISPLAY=:0" >> ~/.bashrc && \
    echo "export EGL_PLATFORM=surfaceless" >> ~/.bashrc

WORKDIR /root