FROM nvidia/cuda:12.6.3-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# 安装系统工具
RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common vim build-essential \
    gcc git zip unzip wget curl htop libgl1 libglib2.0-0 libpython3-dev gnupg libsm6 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN apt upgrade --no-install-recommends -y openssl tar

# 拷贝本地 Miniforge 安装包到容器中
COPY Miniforge3-Linux-x86_64.sh /tmp/miniforge.sh

# 安装 Miniforge 到 /opt/conda，静默模式
RUN bash /tmp/miniforge.sh -b -p /opt/conda \
    && rm /tmp/miniforge.sh

# 创建并激活 py38 环境
ENV PATH=/opt/conda/bin:$PATH
RUN conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ &&\
    conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ &&\
    conda config --set show_channel_urls yes &&\
    conda create -n py38 python=3.8 -y && \
    conda clean -afy

# 激活 py38 并安装python包
RUN /bin/bash -c "source /opt/conda/etc/profile.d/conda.sh && \
    conda activate py38 && \
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip config set global.no-cache-dir true && \
    pip install cython numpy opencv-python opencv-contrib-python uv easydict && \
    pip install ultralytics onnx onnxruntime-gpu onnxslim tensorrt albumentations faster-coco-eval wandb && \
    pip install paddlepaddle-gpu==3.0.0 -i https://www.paddlepaddle.org.cn/packages/stable/cu126/ \
    pip install paddleocr x2paddle \
    conda clean -afy && \
    yolo checks "

RUN mkdir -p /root/.config/Ultralytics/
COPY Arial.ttf Arial.Unicode.ttf yolo11n.pt /root/.config/Ultralytics/
COPY test_img .

RUN rm -rf tmp /root/.config/Ultralytics/persistent_cache.json

# 确保每次启动时激活 py38 环境
RUN echo "source /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc
RUN echo "conda activate py38" >> ~/.bashrc

# 设置工作目录
WORKDIR /workspace

# # 复制项目代码
# COPY . .

# 建立日志目录
RUN mkdir -p log
VOLUME ["/workspace/log"]

# 暴露服务端口
EXPOSE 39527

# 默认进入 bash
CMD ["bash", "-i", "-c", "conda activate py38 && exec bash"]
