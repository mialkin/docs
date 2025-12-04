# kcat

[↑ kcat](https://github.com/edenhill/kcat) is a generic non-JVM producer and consumer for Apache Kafka.

Installation:

```bash
brew install kcat
```

Set environment variables:

```bash
export KAFKA_BROKER=test:9092 \
export KAFKA_TOPIC=test \
export KAFKA_USERNAME=test \
export KAFKA_PASSWORD=test
```

List metadata to make sure connection is working:

```bash
kcat \
-b $KAFKA_BROKER \
-L \
-J \
-X security.protocol=SASL_PLAINTEXT \
-X sasl.mechanisms=SCRAM-SHA-512 \
-X sasl.username=$KAFKA_USERNAME \
-X sasl.password=$KAFKA_PASSWORD
```

```text
-b Broker
-J — Output with JSON envelope
-L — Metadata List
-t — Topic
-P — Produce
-k — Message key
-H — Header
-X — Configuration property
```

Produce JSON with `00000001234` key and a couple of headers:

```bash
kcat \
-b $KAFKA_BROKER \
-t $KAFKA_TOPIC \
-k "00000001234" \
-H "guid=a2c63e41-2795-4a84-a783-fcb01367c889" \
-H "version=1" \
-X security.protocol=SASL_PLAINTEXT \
-X sasl.mechanisms=SCRAM-SHA-512 \
-X sasl.username=$KAFKA_USERNAME \
-X sasl.password=$KAFKA_PASSWORD \
-P <<EOF
{ "requestId": "00000001234", "code": "ABC",  "operation": "XYZ" }
EOF
```
