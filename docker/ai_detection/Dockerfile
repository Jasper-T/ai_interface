FROM nvidia/cuda:12.6.3-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# 安装系统工具并进行升级
RUN apt-get update && apt-get install -y \
    software-properties-common vim build-essential libstdc++6 \
    gcc git zip unzip wget curl htop libgl1 libglib2.0-0 libpython3-dev gnupg libsm6 \
    openssl tar && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# 拷贝本地 Miniforge 安装包到容器中并安装 Miniforge
COPY Miniforge3-Linux-x86_64.sh /tmp/miniforge.sh
RUN bash /tmp/miniforge.sh -b -p /opt/conda && rm /tmp/miniforge.sh

# 创建并激活 py38 环境
ENV PATH=/opt/conda/bin:$PATH
RUN conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ && \
    conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ && \
    conda config --set show_channel_urls yes && \
    conda create -n py38 python=3.8 -y && \
    conda clean -afy

# 拷贝本地 .whl 文件并安装
COPY ./paddlepaddle_gpu-2.5.2.post120-cp38-cp38-linux_x86_64.whl /temp/
RUN /bin/bash -c "source /opt/conda/etc/profile.d/conda.sh && \
    conda activate py38 && \
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip config set global.no-cache-dir true && \
    pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    pip install --default-timeout=9999 cython numpy opencv-python opencv-contrib-python uv easydict loguru ultralytics onnx onnxruntime-gpu onnxslim tensorrt albumentations faster-coco-eval wandb && \
    pip install flask openai && \
    pip install /temp/paddlepaddle_gpu-2.5.2.post120-cp38-cp38-linux_x86_64.whl && \
    pip install paddleocr==2.9.1 && \
    conda clean -afy && \
    rm -f /temp/paddlepaddle_gpu-2.5.2.post120-cp38-cp38-linux_x86_64.whl"

RUN /bin/bash -c "cd /opt/conda/envs/py38/lib && \
    mv libstdc++.so.6 libstdc++.so.6.old && \
    ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 libstdc++.so.6   "
    

# 设置工作目录
WORKDIR /workspace

# 拷贝必要的文件
RUN mkdir -p /root/.config/Ultralytics/
COPY dependencies/Arial.ttf dependencies/Arial.Unicode.ttf /root/.config/Ultralytics/

# 删除临时文件
RUN rm -rf tmp /root/.config/Ultralytics/persistent_cache.json

# 确保每次启动时激活 py38 环境
RUN echo "source /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate py38" >> ~/.bashrc

# 创建日志目录
RUN mkdir -p log
VOLUME ["/workspace/log"]

# 暴露服务端口
EXPOSE 39527

# 默认进入 bash
CMD ["bash", "-i", "-c", "conda activate py38 && exec bash"]
