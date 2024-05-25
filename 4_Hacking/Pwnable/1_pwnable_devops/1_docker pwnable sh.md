docker run
```shell
docker run --ulimit core=-1 -dit -v "C:/Users/kyura/내 드라이브/sync_docker":/sync_docker 88sprout/pwncontainer:latest
```

dockerfile
```sh
FROM ubuntu:22.04

# -e 옵션 없이 실행하면 기본값 사용
ENV USER dev
ENV PASS dev2023
ENV sync_dir /sync_docker

# 인터랙티브 대화 없이 패키지 설치
ENV DEBIAN_FRONTEND=noninteractive

# /sync_docker 디렉토리 소유권 변경
RUN chown -R $USER /sync_docker

# 패키지 설치
RUN apt update \
    && apt install -y sudo git python3 gcc gdb gcc-multilib vim pip openvpn man-db

# 사용자 생성
RUN useradd -ms /bin/bash $USER
RUN echo $USER:$PASS | sudo chpasswd
RUN usermod -aG sudo $USER

# pwntools, pwndbg

# pwndbg 필요 요구사항 설치를 위해 sudo에 비밀번호 없이 실행할 수 있도록 설정
RUN echo "$USER ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USER

# 사용자 변경
USER $USER
WORKDIR /home/$USER

# pwndbg 설치
RUN git clone https://github.com/pwndbg/pwndbg && \
    cd pwndbg && \
    ./setup.sh

# pwndbg 한국어 언어 설정
RUN echo "export LANG=C.UTF-8" >> /home/$USER/.bashrc

# root로 전환하여 pwntools 설치
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