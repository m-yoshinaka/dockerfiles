FROM nvidia/cuda:10.2-cudnn8-runtime-ubuntu18.04 AS builder

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=/opt/python37/bin:${PATH}

WORKDIR /root
RUN apt-get update && apt-get install -y --no-install-recommends \
        ca-certificates build-essential curl libbz2-dev libdb-dev libreadline-dev libffi-dev \
        libgdbm-dev liblzma-dev libncursesw5-dev libsqlite3-dev libssl-dev zlib1g-dev uuid-dev tk-dev \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* \
    && curl -LO https://www.python.org/ftp/python/3.7.10/Python-3.7.10.tar.xz \
    && tar xvf Python-3.7.10.tar.xz && rm Python-3.7.10.tar.xz && cd Python-3.7.10 \
    && ./configure --enable-optimizations --prefix=/opt/python37 && make -j8 && make install \
    && curl -kL https://bootstrap.pypa.io/get-pip.py | python3 \
    && pip install torch==1.8.0

FROM nvidia/cuda:10.2-cudnn8-runtime-ubuntu18.04

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive
COPY --from=builder /opt/python37 /opt/python37

RUN apt-get update && apt-get install -y --no-install-recommends sudo build-essential \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*
ARG UID=1000
ARG GID=1000
ARG UNAME=user1
RUN useradd -m -u ${UID} -G sudo ${UNAME} -s /bin/bash -d /home/${UNAME} -m \
    && groupmod -g ${GID} ${UNAME} \
    && chown -R ${UNAME}:${UNAME} /home/${UNAME} \
    && echo "${UNAME} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/${UNAME} \
    && echo "export PATH=/opt/python37/bin:${PATH}" > /etc/profile.d/py37.sh

USER ${UID}
WORKDIR /home/${UNAME}
CMD /bin/bash
