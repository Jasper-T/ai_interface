# Docker Development

## Docker build

```shell
docker build -t ai_detection_py38_cuda12.6.3:v1.0.0 .
```
## Docker test

1. nvidia device test
    ```shell
    docker run --rm --gpus all test:latest bash -c "nvidia-smi && nvcc -V"
    ```
    
2. conda env test
    ```shell
    docker run --rm --gpus all test:latest bash -i -c "conda info && conda list"
    ```

3. ultralytics yolo test
    ```shell
    docker run --rm --gpus all test:latest bash -i -c "yolo checks"
    docker run --rm --gpus all test:latest bash -i -c "yolo detect predict model=./yolo11n.pt source='./bus.jpg'"
    ```

4. paddle-gpu test
    ```python
    import paddle
    from paddleocr import PaddleOCR
    paddle.utils.run_check()
    ```

5. paddleocr test
    ```shell
    docker run --rm --gpus all test:latest bash -i -c "paddleocr ocr -i ./bus.jpg --use_doc_orientation_classify False --use_doc_unwarping False --use_textline_orientation False"
    ```

## Container
```shell
docker run -d --shm-size=32g --runtime=nvidia --privileged --name test -v /data/code/ultralytics/:/data/code/ultralytics/ test:latest sleep infinity
```