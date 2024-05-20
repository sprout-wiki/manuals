기본적으로 Mosquitto는 포트 1883에서 수신하도록 설정되어 있습니다. 따라서 Mosquitto 설정 파일에서 별도로 포트를 지정하지 않으면 기본 포트 1883을 사용하게 됩니다. 하지만 특정 포트를 명시적으로 설정하거나 다른 포트를 사용하려는 경우 설정 파일에 `listener` 지시어를 추가해야 합니다.

### 포트 설정 확인 및 추가

설정 파일 `/etc/mosquitto/mosquitto.conf`에 포트를 설정하려면 다음과 같이 `listener` 지시어를 추가할 수 있습니다:

`listener 1883`

예를 들어, `/etc/mosquitto/mosquitto.conf` 파일을 다음과 같이 수정할 수 있습니다:

```sh
# Place your local configuration in /etc/mosquitto/conf.d/
#
# A full description of the configuration file is at
# /usr/share/doc/mosquitto/examples/mosquitto.conf.example

persistence true
persistence_location /var/lib/mosquitto/

log_dest file /var/log/mosquitto/mosquitto.log

include_dir /etc/mosquitto/conf.d

# Listener 설정 (포트 1883)
listener 1883

```

### 여러 포트에서 수신하려면

여러 포트를 지정하려면 각각의 포트를 `listener` 지시어로 지정할 수 있습니다. 예를 들어, 기본 포트 1883과 TLS 포트 8883을 설정하려면 다음과 같이 설정할 수 있습니다:

`# 기본 포트 1883에서 수신 listener 1883  # TLS 포트 8883에서 수신 (TLS 설정 예시) listener 8883 cafile /path/to/ca.crt certfile /path/to/server.crt keyfile /path/to/server.key`

이 예에서는 포트 1883에서 기본적으로 수신하고, 포트 8883에서 TLS 암호화를 통해 수신하도록 설정합니다.

### 설정 파일 확인 후 Mosquitto 재시작

설정 파일을 수정한 후 Mosquitto 브로커를 재시작하여 변경 사항을 적용합니다. Docker 환경이 아닌 경우 다음 명령을 사용합니다:

`sudo systemctl restart mosquitto`

Docker 환경에서는 다음과 같이 수동으로 Mosquitto를 실행합니다:

`sudo mosquitto -c /etc/mosquitto/mosquitto.conf`

또는 백그라운드에서 실행하려면:

`nohup sudo mosquitto -c /etc/mosquitto/mosquitto.conf &`

이렇게 하면 Mosquitto가 지정된 포트에서 수신을 시작합니다.