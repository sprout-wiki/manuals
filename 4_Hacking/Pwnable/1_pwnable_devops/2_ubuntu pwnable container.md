docker command
```sh
# unlimit core : segfault core dump 생성 허용
docker run --ulimit core=-1 -dit -v "C:/Users/kyura/내 드라이브/sync_docker":/sync_docker --name=core_pwn1 ubuntu:latest
```

entrypoint.sh (느림)
```sh
#!/bin/bash
# chmod +x entrypoint.sh

# 사용자 및 비밀번호 설정
USER="dev"
PASS="dev2023"

# 카카오 미러 서버 설정
sed -i 's/kr.archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list # 한국
sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list # 미국
sed -i 's/ports.ubuntu.com/ftp.harukasan.org/g' /etc/apt/sources.list # 라즈베리 파이

# 패키지 설치
apt update && \
apt install -y sudo git python3 openssh-server gcc gdb gcc-multilib vim pip openvpn man-db &&;

# 유저 추가
useradd -ms /bin/bash $USER && \
echo "$USER:$PASS" | chpasswd && \
usermod -aG sudo $USER;

# sudo 비밀번호 없이 사용하기 위한 설정
echo "$USER ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USER

# pwndbg 설치
su $USER -c 'git clone https://github.com/pwndbg/pwndbg && \
    cd pwndbg && \
    ./setup.sh' && \
# pwndbg unicode error fix
echo "export LANG=C.UTF-8" >> /home/$USER/.bashrc;
```