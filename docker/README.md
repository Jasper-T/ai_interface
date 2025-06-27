# Docker Development

## Docker build

```shell
docker build -t ai_detection_dev:cuda12.6.3_v1.0.0
```
## Docker test

1. nvidia device test
    ```shell
    docker run --rm --gpus all test:v1.0 bash -c "nvidia-smi && nvcc -V"
    ```
2. conda env test
    ```shell
    docker run --rm --gpus all test:v1.0 bash -i -c "conda info && conda list"
    ```
3. ultralytics yolo test
    ```shell
    docker run --rm --gpus all test:v1.0 bash -i -c "yolo checks"
    ```