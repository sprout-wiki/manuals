# docker container 에서 segfault corefile 얻기

>docker container 의 segment fault 발생 시, corefile 은 host 에 생성됨.
>docker container 의 corefile dump 는 기본적으로 허용되어있지 않음.
>허용 절차:
>1) Host 에서 docker container process 에서 segfault 발생 시, corefile 을 생성하도록 허용
>2) docker container 생성 시 corefile 을 생성하도록 허용
>3) Host 에 생성된 corefile 을 docker container 에서 볼 수 있도록 directory bind

>Windows Docker Desktop 에서는 coredump 생성을 지원하지 않음

https://dev.to/mizutani/how-to-get-core-file-of-segmentation-fault-process-in-docker-22ii
https://ddanilov.me/how-to-configure-core-dump-in-docker-container

```sh
# in docker container's host
echo '/tmp/core.%h.%e.%t' > /proc/sys/kernel/core_pattern
ulimit -c unlimited
```

```plain
docker run \
        --init \
        --ulimit core=-1 \
        --mount type=bind,source=/tmp/,target=/tmp/ application:latest
```