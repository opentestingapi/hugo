---
title: "Hello World"
draft: false
weight: 40
---

Let's start with a simple test case example. 
In general a testcase consist of 2 sections, a section describing the input for our system under test (SUT) and a section describing the output validation.
In addition, a few meta information to identify the test case has to be added. With this, a minimal test cases frame looks like:

```json
{
  "testapi": "0.1",
  "id": "hello_world_test",
  "description": "my first hello world test, showing how its working",
  "injections": [
    ...
  ],
  "checks": [
    ...
  ]
}
```
* `testapi` : Version of the opentestapi your test case is written in.
* `id` : A testing tool can manage test cases using this identifier. 
* `description`: We humans usually need some help. Let's add a short description for us.

* **Input**: Test data are injected into an interface, whatever type it is off. So input is just an injection. 
A test can contain several data injects, so its called `injections`. You might prefer the word 'event' :-)
* **Output**: The system output has to be validated - we check the out. And we will not do one check, we allow several. So it' `checks`.

## Injections 
```json
    "injections" : [
     {
        "injectid": "inject-kafka-1",
        "service" : {
            "type" : "kafka",
            "connectstring":"localhost:9092",          
            "custom": {
                "topic": "mytopic"
            },
        },
        "checks" : [ "check-kafka-1" ],        
        "cron" : [ "0 * * * * ?"],
        "sourcefile" : "./hello_world_test/kafka_inject_data_1.txt"
     }]
```
* `injectid`: An identifier, e.g. to relate this inject-data with other actions
* `service`: The `type` of the interface you to want to inject the test data. In our example it's a `kafka` service. `broker` and topic describes the technical connection to this interface, authentication is also possible.
* `checks`: A list of checks, which should be performed after this inject was triggered
* `cron`: The time, when the test has to be executed. Yes, the time!  
Usually, a test case will be fired and forget. In general, this is also possible (simple ignore the cron - it's not mandatory).
We support this **cron** approach in order to have continuous testing - E2E systems should run continuously and also continuously tested.
* `sourcefile`: The most important one! Your test data, which has to be injected into the input interface. 
In the hello world example the file 'source_kafka_1.txt' contains 
```text
hello world
```
## Checks
Let's now check, if your system under test processed everything in the right way. 
Let's assume the system is loading the data from Kafka inverts the input and write it to another topic.
The corresponding check looks as follows:
```json
"checks": [
  {
      "checkid" : "check-kafka-1",
      "service" : {
          "type" : "kafka",
          "connectstring":"localhost:9092",
          "custom": {
              "topic": "mytopic",
              "group": "mygroup"
          }
      },
      "expectedfile" : "./hello_world_test/kafka_check_data_1.txt",
      "checktype": "contains",      
      "maxwaittime" : "10m"
  }]
```
* `checkid`: The identifier for this check
* `service`: The interface, which has to be checked. The approach is the same as on the inject.
* `expectedfile`: The expected output of the process is written in "check_kafka_1.txt". As mentioned, we expect the topic contains once the following message:
```text
dlrow olleh
```
* `checktype`: We expect that the message inside the topic looks like the one from the expected file. The kafka message should `contains` our text.
* `maxwaittime`: How to check in a (real) real environment, where several process can run. 
We can not say, when the SUT will process our input. We only can say, it should not take longer as a certain time. 
 So, we have to define this maxtime to expect our date.   

Finally, your testcase will look as follows on your local environment:
```shell
./hello_world_test.JSON
./hello_world_test/
./hello_world_test/kafka_inject_data_1.txt
./hello_world_test/kafka_check_data_1.txt
```
