# Schema Registry

Create topic in Kakfa

```bash
kafka-topics --create --topic click_avro --bootstrap-server localhost:9092
```

Find the schema Id

```
SUBJECT=click_avro
VERSION=1
SCHEMA_ID=$(curl -s "http://localhost:18081/subjects/${SUBJECT}-value/versions/${VERSION}" | jq '.id')
echo $SCHEMA_ID
```

Publish messages

```bash

# change $SCHEMA_ID to schema id from previous command

kafka-avro-console-producer \
  --broker-list kafka:19092 \
  --topic click_avro \
  --property schema.registry.url=http://localhost:8081 \
  --property value.schema.id=$SCHEMA_ID
  
# with message like

{"itemId": "item123", "count": 1, "eventTime": 1623259315000}
```

Consumer messages

```bash
kafka-avro-console-consumer   \
  --bootstrap-server kafka:19092 \
  --topic click_avro \
  --property schema.registry.url=http://localhost:8081 \
  --property print.key=true \
  --from-beginning
  
kafka-avro-console-consumer   \
  --bootstrap-server kafka:19092 \
  --topic click_avro \
  --property schema.registry.url=http://localhost:8081 \
  --from-beginning
```
