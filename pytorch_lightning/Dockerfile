FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential cmake git tmux wget bash-completion \
    curl vim ca-certificates sudo openssh-server  \
    python3 python3-dev python3-distutils less \
    && apt-get autoremove -y && apt-get autoclean -y \
    && mkdir /var/run/sshd
RUN curl -LO https://bootstrap.pypa.io/get-pip.py \
    && python3 get-pip.py \
    && rm get-pip.py \
    && pip3 install torch==1.5.1+cu101 torchvision==0.6.1+cu101 \
    -f https://download.pytorch.org/whl/torch_stable.html \
    && pip3 install scikit-learn pandas pytorch-lightning \
    docopt flake8 autopep8 jupyterlab

ARG UID=1000
ARG UNAME=docker
ARG PASSWD=docker-user
RUN useradd -m -u ${UID} -G sudo ${UNAME} -s /bin/bash -d /home/${UNAME} -m \
    && echo ${UNAME}:${PASSWD} | chpasswd \
    && chown -R ${UNAME} /home/${UNAME} \
    && chsh -s /bin/bash
USER ${UID}
WORKDIR /home/${UNAME}
RUN mkdir .ssh && chmod 700 .ssh \
    && touch .ssh/authorized_keys \
    && chmod 600 .ssh/authorized_keys

CMD /bin/bash
