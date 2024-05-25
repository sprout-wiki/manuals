docker run
```shell
docker run --ulimit core=-1 -dit -v "C:/Users/kyura/내 드라이브/sync_docker":/sync_docker 88sprout/pwncontainer:latest
```

dockerfile
```sh
FROM ubuntu:22.04

# run without -e option then default
ENV USER dev
ENV PASS dev2023
ENV sync_dir /sync_docker

ENV DEBIAN_FRONTEND=noninteractive

RUN chown -R dev /sync_docker

RUN apt update \
    && apt install -y sudo git python3 gcc gdb gcc-multilib vim pip openvpn man-db

RUN useradd -ms /bin/bash $USER
RUN echo $USER:$PASS | sudo chpasswd

RUN usermod -aG sudo $USER

# pwntools, pwndbg

# set no password to sudo to install pwndbg requirements
RUN echo "$USER ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USER

USER $USER
WORKDIR /home/$USER

RUN git clone https://github.com/pwndbg/pwndbg && \
    cd pwndbg && \
    ./setup.sh

# fix pwndbg lang formatting
RUN echo "export LANG=C.UTF-8" >> /home/$USER/.bashrc

USER root
RUN pip install pwntools
```

docker build and push
```sh
# Should Locate file name=Dockerfile
docker build -t 88sprout/pwncontainer:latest .
docker login
docker push 88sprout/pwncontainer:latest
```

shell command (apt download 가 너무 느리긴 함)
```sh
apt update \
    && apt install -y sudo git python3 openssh-server gcc gdb gcc-multilib vim pip openvpn man-db && \

useradd -ms /bin/bash dev && \
echo "dev:dev2023" | chpasswd && \
usermod -aG sudo dev && \
su dev && \


git clone https://github.com/pwndbg/pwndbg && \
    cd pwndbg && \
    ./setup.sh \ &&
echo "export LANG=C.UTF-8" >> /home/$USER/.bashrc;
```