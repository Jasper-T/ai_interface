# Docker Development

## Docker build

```shell
docker build -t ai_detection_py38_cuda12.6.3:v1.0.0 .
```

## Container
```shell
docker run -d -p 9527:8881 --shm-size=32g --gpus "device=0" --privileged --name test -v /data/code/ultralytics/:/data/code/ultralytics/ ai_detection_py38_cuda12.6.3:v1.0.0 sleep infinity
```

## App 
```shell
# nohup /opt/conda/envs/py38/bin/python -m gunicorn -c /data/code/ultralytics/aiconfig.py serive:app &
nohup python serive.py > tjp.log 2>&1 &
```

## Save
```shell
docker save -o ai_detection_py38_cuda12.6.3_v1.0.0.tar ai_detection_py38_cuda12.6.3:v1.0.0
```