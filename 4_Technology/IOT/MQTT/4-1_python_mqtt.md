
#### MQTT Publisher
```python
import paho.mqtt.client as mqtt

broker_address = "broker.hivemq.com"
client = mqtt.Client("Publisher")
client.connect(broker_address, 1883, 60)

client.publish("test/topic", "Hello MQTT")
client.disconnect()

```

#### MQTT Subscriber
```python
import paho.mqtt.client as mqtt

def on_message(client, userdata, message):
    print(f"Message received: {message.payload.decode()}")

broker_address = "broker.hivemq.com"
client = mqtt.Client("Subscriber")
client.connect(broker_address, 1883, 60)

client.subscribe("test/topic")
client.on_message = on_message

client.loop_forever()

```