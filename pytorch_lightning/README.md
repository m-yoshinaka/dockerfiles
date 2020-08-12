Dockerfile for PyTorch-Lightning

- PyTorch: 1.5
- CUDA: 10.1
- cuDNN: 7
- ubuntu18.04

```
$ docker build --build-arg UID=${UID} -t pytorch-lightning:pytorch1.5-cuda10.1 - < Dockerfile
```
