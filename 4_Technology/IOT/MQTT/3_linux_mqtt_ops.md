
apt-add-repository 를 사용하기 위한 설치
```sh
sudo apt-get update
sudo apt-get install software-properties-common
```

```shell
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt-get update
sudo apt-get install mosquitto mosquitto-clients
sudo systemctl status mosquitto
sudo systemctl start mosquitto

sudo systemctl enable mosquitto # 재부팅 시 자동시작 활성화
```

세팅 파일
```
/etc/mosquitto/mosquitto.conf
```

도커여서 systemctl 을 사용할 수 없는 경우, 바이너리를 직접 실행
nohup 안쓰려면 superviser 나 systemd 로 묶어야 할 듯.
```sh
sudo mosquitto -c /etc/mosquitto/mosquitto.conf
nohup sudo mosquitto -c /etc/mosquitto/mosquitto.conf &
```

### Mosquitto 클라이언트 사용 예제
Mosquitto 클라이언트 도구인 `mosquitto_pub`와 `mosquitto_sub`를 사용하여 브로커가 제대로 작동하는지 테스트할 수 있습니다.

아래 서버가 구독(subscribe) 인 상태에서,
```sh
mosquitto_sub -h localhost -t test/topic
```

클라이언트의 publish 를 실행하면,
```sh
mosquitto_pub -h localhost -t test/topic -m "Hello, MQTT"
```

아래와 같이 표시됨
![](image1.png)

### Mosquitto TLS 설정

인증서 생성
```sh
# CA 인증서 생성
openssl req -new -x509 -days 365 -extensions v3_ca -keyout ca.key -out ca.crt

# 서버 인증서 생성
openssl genpkey -algorithm RSA -out server.key
openssl req -new -key server.key -out server.csr
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 365

# 클라이언트 인증서 생성
openssl genpkey -algorithm RSA -out client.key
openssl req -new -key client.key -out client.csr
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 365

```

Mosquitto 설정파일 수정
```sh
listener 8883
cafile /etc/mosquitto/certs/ca.crt
certfile /etc/mosquitto/certs/server.crt
keyfile /etc/mosquitto/certs/server.key
tls_version tlsv1.2
```

생성한 인증서 파일들을 `/etc/mosquitto/certs/` 디렉토리에 복사합니다.
```sh
sudo mkdir -p /etc/mosquitto/certs
sudo cp ca.crt server.crt server.key /etc/mosquitto/certs/
```

Mosquitto 브로커를 재시작하여 TLS 설정을 적용합니다.
```sh
sudo systemctl restart mosquitto
```