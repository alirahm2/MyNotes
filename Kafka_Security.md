## SCRAM
```
sasl.enabled.mechanisms=SCRAM-SHA-256
sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256
security.inter.broker.protocol=SASL_PLAINTEXT
listeners=SASL_PLAINTEXT://0.0.0.0:9093
advertised.listeners=SASL_PLAINTEXT://localhost:9093
```

## PLAN and SCRAM
```
sasl.enabled.mechanisms=SCRAM-SHA-256,PLAIN
sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256
security.inter.broker.protocol=SASL_PLAINTEXT
listeners=SASL_PLAINTEXT://0.0.0.0:9093,PLAINTEXT://0.0.0.0:9092
advertised.listeners=SASL_PLAINTEXT://localhost:9093,PLAINTEXT://localhost:9092
```

## Create user and password
```
bin/kafka-configs.sh --zookeeper localhost:2181 --alter --add-config 'SCRAM-SHA-256=[iterations=8192,password=done],
SCRAM-SHA-512=[password=done]' --entity-type users --entity-name done
```

## Python API Example - Producer - PLAN
```python
import time

from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers="localhost:9092")

for i in range(10):
    millis = str(int(round(time.time() * 1000)))
    producer.send("jasem", bytes(millis, "utf-8"))
    producer.flush()

```

## Python API Example - Producer - SCRAM
```python
import time

from kafka import KafkaProducer
import ssl

context = ssl.create_default_context()
context.options &= ssl.OP_NO_TLSv1
context.options &= ssl.OP_NO_TLSv1_1

producer = KafkaProducer(bootstrap_servers="localhost:9093",
                         security_protocol='SASL_PLAINTEXT',
                         sasl_mechanism='SCRAM-SHA-256',
                         sasl_plain_username='done',
                         sasl_plain_password='done',
                         ssl_context=context,
                         api_version=(0, 10),
                         retries=5)

for i in range(10):
    millis = str(int(round(time.time() * 1000)))
    producer.send("jasem", bytes(millis, "utf-8"))
    producer.flush()
```


## Python API Example - Consumer- PLAN
```python
import time
from kafka import KafkaConsumer

consumer = KafkaConsumer("jasem",
                         bootstrap_servers="localhost:9092",
                         group_id="jasemwwwww"
                         )
while True:
    for i in consumer:
        start = int(i.value)
        end = int(round(time.time() * 1000))
        print(end - start)
    time.sleep(1)

```


## Python API Example - Consumer- SCRAM
```python
import time
from kafka import KafkaConsumer
import ssl

context = ssl.create_default_context()
context.options &= ssl.OP_NO_TLSv1
context.options &= ssl.OP_NO_TLSv1_1

consumer = KafkaConsumer("jasem",
                         bootstrap_servers="localhost:9093",
                         security_protocol='SASL_PLAINTEXT',
                         sasl_mechanism='SCRAM-SHA-256',
                         sasl_plain_username='jasem',
                         sasl_plain_password='jasem-secret',
                         ssl_context=context,
                         api_version=(0, 10),
                         group_id="jasemwwwww"
                         )
while True:
    for i in consumer:
        start = int(i.value)
        end = int(round(time.time() * 1000))
        print(end - start)
    time.sleep(1)

```


