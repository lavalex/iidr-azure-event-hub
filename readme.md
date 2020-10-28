# IBM InfoSphere Data Replication to Azure Event Hub


**kafkacat** configuration:

Configuration file:

```metadata.broker.list=<namespace>.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanisms=PLAIN
sasl.username=$ConnectionString
sasl.password=Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<Key name>;SharedAccessKey=<Access key>
```

To set the default configuration file:

```export KAFKACAT_CONFIG=<full path to config file>```


**kafka-console-producer** and **kafka-console-consumer** configuration:

```
bootstrap.servers=<namespace>.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<Key name>;SharedAccessKey=<Access key>";
```

Specify the configuration file using **--producer.config** or **--consumer.config** properties


** IBM Change Data Capture ** configuration:

1. Create **jaas.conf** file:

```
KafkaClient {
    org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<Key name>;SharedAccessKey=<Access key>";
};
```

2. Specify it in **<cdc path>/instance/<instance name>/conf/dmts64.vmargs** file:

```
-Djava.security.auth.login.config=<full path to jaas.conf>
```

3. Modify **<cdc path>/instance/<instance name>/conf/kafkaproducer.properties** file and add:
  
```
metadata.broker.list=<namespace>.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanisms=PLAIN
```

4. Modify **<cdc path>/instance/<instance name>/conf/kafkaconsumer.properties** file and add:
  
```
metadata.broker.list=<namespace>.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanisms=PLAIN
```

Restart the instance if it's running.
