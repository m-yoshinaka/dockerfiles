# Dockerfile for PyTorch 1.8.0

- Base image: `nvidia/cuda:10.2-cudnn8-runtime-ubuntu18.04`
- Python: 3.7.10 (Path=/opt/python37/bin)
- PyTorch: 1.8.0

## Build image
```
$ docker build --build-arg UID=$UID --build-arg GID=$(id -g) \
    -t foo/pytorch:1.8.0-cuda10.2-py3.7 - < Dockerfile
```

## Run container
```
$ docker run -it --rm --ipc=host --gpus=all --name bar \
    foo/pytorch:1.8.0-cuda10.2-py3.7 /bin/bash --login
```
