# offset in kafka

## 调用实例

``` java
    @Test
    public void testKafka(){
        String topic = "spider-task";
        /*
        如果kafkaConfMap设置了group_id,SparkStreamingKafka可不设置group_id
         */
//        String groupId = "spark-template";
        Map<Object,Object> sparkConfMap = new HashMap<>();
        sparkConfMap.put(TemplateConf.APP_NAME,"testMysql");
        sparkConfMap.put(TemplateConf.MASTER,"local[4]");
        sparkConfMap.put(TemplateConf.DURATION, Durations.seconds(10));
        Map<String,Object> kafkaConfMap = new HashMap<>();
        kafkaConfMap.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "192.168.2.58:9092,192.168.2.58:10092,192.168.2.58:11092");
        kafkaConfMap.put(ConsumerConfig.GROUP_ID_CONFIG, "spark-template");
        kafkaConfMap.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        kafkaConfMap.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        kafkaConfMap.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        kafkaConfMap.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        SparkStreamingKafka spark = SparkStreamingKafka.create(sparkConfMap, kafkaConfMap)
                .setTopicName(topic)
                .setOffsetTemplate(new OffsetInKafkaTemplate(kafkaConfMap));
        spark.start();
    }
```
在传入SparkConfMap和KafkaConfMap之后，set offset的存储模板为kafka模板(OffsetInKafkaTemplate),传入KafkaConfMap以用于创建kafka连接