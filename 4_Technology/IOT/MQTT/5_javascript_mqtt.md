
- python 과 달리, mqtt:// 라는 url scheme 이 필요함. (mqtt 프로토콜을 사용함을 명시.)
- python 과 달리, mqtt 객체는 publish 와 subscribe 둘다 수행할 수 있음 (메서드로 되어있음)


```javascript
var mqtt = require('mqtt');
var client  = mqtt.connect('mqtt://broker.hivemq.com');

// 클라이언트가 연결되었을 때
client.on('connect', function () {
  console.log('Connected');
  client.subscribe('test/topic', function (err) {
    if (!err) {
      client.publish('test/topic', 'Hello MQTT');
    }
  });
});

// 메시지를 받을 때
client.on('message', function (topic, message) {
  // 메시지는 버퍼 형식으로 옵니다.
  console.log(message.toString());
  client.end();
});

```
