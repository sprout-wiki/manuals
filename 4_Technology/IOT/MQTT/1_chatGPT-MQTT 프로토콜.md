IoT 기기와 서버 간의 통신에서 Publisher와 Subscriber의 역할을 이해하려면 MQTT 프로토콜의 발행/구독 모델을 어떻게 활용하는지 살펴보아야 합니다.

### Publisher (발행자)

**Publisher**는 데이터를 생성하여 특정 주제(topic)에 발행하는 역할을 합니다. IoT 환경에서는 다양한 기기가 데이터를 생성하고 이를 다른 기기나 서버에 전달해야 하는 경우가 많습니다.

#### IoT 기기에서 Publisher의 역할:

- **센서 데이터 발행**: 온도, 습도, 조도 등 센서 데이터를 주기적으로 수집하여 MQTT 브로커에 발행합니다.
- **상태 정보 발행**: 장치의 상태 변화(예: 전원 상태, 연결 상태)를 발행합니다.
- **이벤트 알림 발행**: 특정 이벤트 발생 시(예: 문 열림 감지, 움직임 감지) 해당 이벤트 정보를 발행합니다.

예를 들어, 온도 센서가 일정 시간 간격으로 현재 온도를 `sensor/temperature` 주제에 발행할 수 있습니다.

### Subscriber (구독자)

**Subscriber**는 특정 주제를 구독하여 해당 주제에 발행된 메시지를 수신하는 역할을 합니다. 이를 통해 필요한 데이터를 실시간으로 받아 처리할 수 있습니다.

#### 서버에서 Subscriber의 역할:

- **데이터 수집**: 다양한 IoT 기기에서 발행하는 센서 데이터를 실시간으로 수집합니다.
- **상태 모니터링**: IoT 기기의 상태를 모니터링하고 필요한 경우 알림을 보냅니다.
- **이벤트 처리**: 특정 이벤트가 발생했을 때 대응하는 작업을 수행합니다(예: 경고 메시지 전송, 데이터 저장).

예를 들어, 서버는 `sensor/temperature` 주제를 구독하여 모든 온도 센서의 데이터를 실시간으로 수신하고 저장할 수 있습니다.

### 전체 시스템에서의 역할

#### 예시 시나리오:

1. **IoT 기기(온도 센서)**:
    
    - 온도 데이터를 주기적으로 측정하고 `sensor/temperature` 주제에 발행합니다.
    
    python
    
    코드 복사
    
    `import paho.mqtt.client as mqtt import time  broker_address = "broker.hivemq.com" topic = "sensor/temperature"  client = mqtt.Client("TemperatureSensor") client.connect(broker_address)  while True:     temperature = get_temperature()  # 온도 측정 함수     client.publish(topic, temperature)     time.sleep(60)  # 1분마다 발행`
    
2. **서버**:
    
    - `sensor/temperature` 주제를 구독하여 모든 온도 데이터를 실시간으로 수신하고 데이터베이스에 저장합니다.
    
    python
    
    코드 복사
    
    `import paho.mqtt.client as mqtt  broker_address = "broker.hivemq.com" topic = "sensor/temperature"  def on_connect(client, userdata, flags, rc):     if rc == 0:         print("Connected to broker")         client.subscribe(topic)     else:         print("Connection failed with code", rc)  def on_message(client, userdata, message):     temperature = message.payload.decode()     save_to_database(temperature)  # 데이터베이스 저장 함수  client = mqtt.Client("Server") client.on_connect = on_connect client.on_message = on_message client.connect(broker_address) client.loop_forever()`
    

### 결론

- **Publisher**: 데이터를 생성하고 발행하는 역할을 담당합니다. IoT 기기가 주로 이 역할을 수행하여 센서 데이터, 상태 정보, 이벤트 알림 등을 발행합니다.
- **Subscriber**: 특정 주제를 구독하여 데이터를 수신하는 역할을 담당합니다. 서버가 주로 이 역할을 수행하여 IoT 기기에서 발행한 데이터를 수집하고 처리합니다.

이러한 발행/구독 모델을 통해 IoT 시스템은 확장성과 유연성을 갖추게 되며, 각 구성 요소가 독립적으로 동작하면서도 실시간으로 데이터를 교환할 수 있게 됩니다.