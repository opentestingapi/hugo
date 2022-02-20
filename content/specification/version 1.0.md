---
date: 2021-10-11
linktitle: Version 1.0
menu:
  main:
    parent: Specification
title: Version 1.0
weight: 10
---

# OpenTestAPI Specification

<a href="../OpenTestApiSchema_v1.0.json">[Download JSON Schema Specification]</a>

## Disclaimer
Part of this content has been taken from the great work done by the folks at the
<a href="https://www.asyncapi.com/docs/getting-started">[AsyncAPI]</a> 
initiative and the 
<a href="https://github.com/OAI/OpenAPI-Specification">[OpenAPI]</a> initiative.

## Introduction

The OpenTestAPI specification is a project to describe and document tests cases in a machine-(and human-)readable format. 
The specification defines a set of files to describe a test case. 
Creation and execution of the test cases can be done by utilities. 

## Schema Specification
### OpenTestAPI Object

This is the root document object for the API specification. 
It combines resource listing and API declaration together into one document.

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
testapi | [Version String]({{< ref "#testapi-version-string" >}}) | **Required** Specifies the TestAPI version. Can be used by clients to interpret the version. 
id | [Identifier]({{< ref "#identifier" >}}) | Identifier of the test case, which will be delivered.   
description | String | The informal description of the test case.  
injections | [[Injection Object]]({{< ref "#injections-object" >}}) | The set of injection objects, part of this testcase.
checks | [[Check Object]]({{< ref "#check-object" >}}) | The set of check objects, part of this testcase.  
labels | String Array | labels applied to the test case, used for additional features like search

### TestAPI Version String
The version string signifies the version of the TestAPI Specification that the document complies to. 
The format for this string must be major.minor. 

A major.minor shall be used to designate the TestAPI Specification version, and will be considered compatible with the TestAPI Specification specified by that major.minor version. 

In subsequent versions of the TestAPI Specification, care will be given such that increments of the minor version should not interfere with operations of tooling developed to a lower minor version. 
Thus a hypothetical 1.1 specification should be usable with tooling designed for 1.0.

### Identifier
This field represents a unique universal identifier.
The format is in general free, but it can be used to package test cases to testsuites.
Good practice is to follow the URI format, according to <a href="https://tools.ietf.org/html/rfc3986">RFC3986</a>.

**EXAMPLE**
```json
{
    "id": "myproject:myexampleapplication:E2E:test1" 
}
```
```json
{
    "id": "myproject:myexampleapplication:E2E:test1:kafka:inject:mymessage" 
}
```

### Injection Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
injectid |  [Identifier]({{< ref "#identifier" >}}) | **Required** A unique Identifier, which allows to connect an inject with several checks.   
service |  [Service Object]({{< ref "#service-object" >}})  | **Required** the service (interfaces), which will be used to inject the testing data. 
checks | [[Checks]]({{< ref "#checks" >}}) | A set of `checkids`, describing the checks executed after the inject was triggered. 
cron | [[Cron Trigger]]({{< ref "#cron-trigger" >}}) | The execution time of a job
timetolive | [Time Duration Object]({{< ref "#time-duration-object" >}}) | Based on the continuous execution approach, a test case will be executed endless. `timtolive` defines how long the test cases has to be executed by the test tool once it was started. If nothing is defined, the assumption will be that the test cases is executed endless based on the cron job. 
sourcefile | [Filename]({{< ref "#filename" >}}) | The data source for the inject. The complete data of this file will be taken ans input for the SUT.
replacement | [Replacement Object]({{< ref "#replacement-object" >}}) | It's possible to inject random generated data into the input object. If the testing tool supports the feature, it is also possible to trace the random generated values through the test case. That is, once a random value was generated, it can be used inside the output check for validation.  

### Service Object

A SUT can have several interfaces for interaction. The OpenTestAPI defines a common approach to interact with different service interfaces. 
Therefore, OpenTestingAPI defines a common and a custom configuration part. The custom configuration  

The common service configuration part looks as follows

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
type | String | **Required** The definition of the service type.
connectstring | String | **Required** The connect string for the service. Usually, this will be an Url. However, its also possible to interact via other interfaces, e.g. files.
user | String | In case of authentication, the username.
password | Base64String | The password for the authentication.
custom | [(String, String)] | A service can have custom configurations, e.g. topics etc. The custom list allows the simple configuration via key/value pairs. 

At this moment, the OpenTestAPI supports the following services:

| Service    | Type-String | DESCRIPTION
------------ | ------------- | -------------
[Kafka]({{< ref "#kafka-service-object" >}}) | kafka| <a href="https://kafka.apache.org/">Apache Kafka</a> is an often used technology for asynchronous communication and data stream processing. The SUT communicates via topics with its environments.
[JDBC Database]({{< ref "#jdbc-database-object" >}}) | jdbc | Connect to JDBC Databases, e.g. PostgreSQL, MariaDB, Oracle DB 
[Cassandra Database]({{< ref "#cassandra-database-object" >}}) | cassandra | Connect to Cassandra Database 
[REST]({{< ref "#rest-service-object" >}}) | rest | Connect a service via HTTP Requests.

<b>ATTENTION: Do NOT use any technologies effecting business process, for example queues!</b>


### Kafka Service Object

Addition / Custom Fields:

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
topic | String | **Required** The topic to inject data / consumer data from.
group | String | Kafka uses consumer groups to cooperate consumers of one topic. The SUT and the testing tool will consume data from the same topic. For this, a dedicated testing group is required. 


**EXAMPLE**
```json
{
    "service": {
        "type": "kafka",
        "connectstring":"mykafkabroker.com",
        "username": "myusername",
        "password": "dW5zZWN1cmUgcGFzc3dvcmQ=",        
        "custom": [
            {
                "key": "topic",
                "value": "mytopic"
            },
            {
                "key": "group",
                "value": "mygroup"
            }
        ]             
    }
}
```

### JDBC Database Object

The connectstring for SQL databases follows the <a href="https://docs.oracle.com/cd/E17952_01/connector-j-8.0-en/connector-j-reference-jdbc-url-format.html">JDBC Url</a> syntax.
The Url string consists of a protocol-, host-, database- and properties definition. 

**EXAMPLE**
```json
{
  "service": {
        "type": "jdbc",
        "connectstring":"jdbc:oracle:thin:@localhost:1521/XE",
        "username": "myusername",
        "password": "dW5zZWN1cmUgcGFzc3dvcmQ="    
    }
}
```

### Cassandra Database Object

Please use server:port to specify the target cluster.

**EXAMPLE**
```json
{
  "service": {
        "type": "cassandra",
        "connectstring":"cassandra-db:9042",
        "username": "myusername",
        "password": "dW5zZWN1cmUgcGFzc3dvcmQ="    
    }
}
```

### REST Service Object

Addition / Custom Fields:

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
method | String | HTTP-method, which has to be performed, e.g. GET, POST (see <a href="https://datatracker.ietf.org/doc/html/rfc7231#section-4.3">rfc7231</a>).

**EXAMPLE**
```json
{
  "service": {
        "type": "rest",
        "connectstring":"http://localhost:50000/dummy/post",
        "username": "myusername",
        "password": "dW5zZWN1cmUgcGFzc3dvcmQ=",
        "custom": [
            {
                "key": "method",
                "value": "post"
            }
        ]   
  }
}
```

### Check Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
checkid | [Identifier]({{< ref "#identifier" >}}) | **Required** A unique ID for the check.
service | [Services]({{< ref "#service" >}})  | **Required** the service (interfaces), which will be used to check the testing data.
validations | [[Validation Object]]({{< ref "#validation" >}})  | **Required** the validation, which define the check
maxwaitime | [Time Duration Object]({{< ref "#time-duration-object" >}}) | **Required** This defines the maximum time a testing tools has to wait before it marks a check as failed. If the expected output arrives in the timeframe (inject-start, maxwaitime) the test will be reported as success.
active | String | activate (TRUE) /deactivate (FALSE) a check. Per default a testing tool will set a check always as activated and execute it once, the inject referenced it.

### Validation Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
order | Number | A JSON List object has per default no order. In order to sort the sub value generation an order ID is introduced. This parameter is optional.
request | [Filename]({{< ref "#filename" >}}) | The file contains the request object (inside a file), which has to be executed on the inject interface. In case of a SQL DB System its an SQL query. 
response | [[Filename]({{< ref "#filename" >}})] | **Required** The data source for the expected response object. 
type | String | **Required** Check type defines, how to perform the output validation. Valid values are EQUALS, CONTAINS, CONTAINSNOT, CONTAINSONEOF... . See [Check Type Definition]({{< ref "#check-type-definition" >}})

**EXAMPLE**
```json
{
  "validations" : [{
        "order": 1,
        "request": "check_postgresql_param_1.sql",
        "type": "containsOneOf",
        "response": [ "expected_result_test1.txt", "expected_result_test2.txt"]        
      }
  ]
}
```

### Checks

A set of `checkids`, describing the checks executed after the inject was triggered.

**EXAMPLE**
```json
{
    "checks": [ "kafka:check1", "kafka:check2", "oracle:check1"]
}
```




### Check Type Definition
The Open-Test-API supports the following types of validation:
CHECK    |  DESCRIPTION
---------|  -------------
EQUALS | The testing tool reports success if the output is equal to the expected output. 
CONTAINS | The testing tool reports success if all output files contain valid output. 
CONTAINSNOT | The testing tool reports success if the output does not contain the expected output.
CONTAINSONEOF | The testing tool reports success if one of the response files contains the expected output

### Cron Trigger

The Cron Trigger is a comma-separated list of cron jobs. The cron job definition follows the UNIX cronjob definition (<a href="https://en.wikipedia.org/wiki/Cron">[Cron Job]</a>)

The following examples will trigger the data inject every hour 5 and 15 minutes after the full hour.  

**EXAMPLE**
```json
{
    "cron" : [ "5 * * * * ?" , "15 * * * * ?"], 
}
```

### Filename

The text content of file will be taken as the input for injects and/or checks. The filename can be name only or relative path to your data source. 
It's a good practice to have the files next to your testcase description, e.g. all data sourcefiles for a testcase are inside a directory together with the testcase.    
Within test case definition you MUST use the filename only, as implementation is not aware of storage type and folders - upload will use filename only!

**EXAMPLE**
With relative path `./`
```
/testcase1/testcasedescription.JSON
/testcase1/inject1.txt
/testcase1/inject2.txt
/testcase1/check1.txt
/testcase1/check2.txt
```
or with relative path `testcase1/`
```
testcasedescription.JSON
/testcase1/inject1.txt
/testcase1/inject2.txt
/testcase1/check1.txt
/testcase1/check2.txt
```
### Time Duration Object

A time duration MUST be a number bigger 0 representing the duration in seconds.
For simplification, the following abbreviations are possible:

Abbreviations | Description
----------- | -------------------
s | seconds
m | minutes
h | hours
d | days

**EXAMPLE**
```json
{
  "maxwaitime": "60",
  "maxwaitime": "60s",
  "maxwaitime": "1m",
}
```

### Replacement Object
It's possible to inject random generated data into the input object and validate the output object for this generated date. 

With this, it's possible to trace objects during a complex SUT overall several subsystems.

A replacement object is a set of  

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
replacements | [[Replacement Rule]]({{< ref "#replacement-rule" >}}) | A set of replacements.

### Replacement Rule
FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
key | String | The key defining the replacement object. The testing tool will look for this key to replace it by the random generated value. 
value | [[Replacement Value]({{< ref "#replacement-value" >}})] | A comma separated list of values for the replacement, the concatenation of this list will be the replacement value.
maxlength | Integer | 

### Replacement value 
The description of a single value looks as follows

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
order | Number | A JSON List object has per default no order. In order to sort the sub value generation an order ID is introduced. This parameter is optional.
type | String | Defines the way how to generate random values. Type and its parameters are described in the next table. 
value | String | Defines the specific definition how to generate random values. 
param | String | Parameter object for the random generator.

The following types are supported by the OpenTestApi Definition

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
now | String | Value will be the current timestamp (+an additional delay) in the given format. Parameter (param) accepts (+[delay]({{< ref "#time-duration-object" >}}))
list | String | List of Pipe ("\|") separated values, which can be picked randomly for the value. The values are grabbed evenly distributed. 
regex | String | Random generated value matching the regular expression
inheritfrom | String | Inherit from different Inject (InjectID as value) with same #key#
file | [Filename]({{< ref "#filename" >}}) | The file contains a set of values, which are randomly chosen. One value per line.


**EXAMPLES**

A simple timestamp example:

```json
"replacements" : [
{
    "key" : "#key1#",
    "value": [{   
        "order": 1,
        "type": "now",    
        "value": "yyyy-MM-dd HH:mm:ss",
        "param" : "+1h"
    }]   
}]
```

This will generate a replacement value, which generates a timestamp with one hour in the future. That is, your inject job will be done at 10am, the generated value will be 11am.

```json
"replacements" : [
{
    "key" : "#key1#",
    "value": [{        
        "order": 1,
        "type": "list",    
        "value": "v1|v2|v3|v5"
    }]   
}]
```
The generator will pick random one of the values out of the list given as param.

```json
"replacements" : [
{
    "key" : "#key1#",
    "value": [{        
        "order": 1,
        "type": "regex",    
        "value": "^[0-9A-F]{8}-[0-9A-F]{4}-4[0-9A-F]{3}-[89AB][0-9A-F]{3}-[0-9A-F]{12}$"
    }],        
}]
```
This regex generates a UUID in format v4.

A more complex example combines several generators:
```json
"replacements" : [
{
    "key" : "#key1#",
    "value": [{
        "order": 1,
        "type": "list",    
        "value": "v1_0|v2_|v3_|v5_00"
    },{
        "order": 2,
        "type": "regex",    
        "value": "^[0-9]{2}$"
    }],
    "maxlength":5
}]
```
This will pick a value out of the list "v1_0, v2_, v3_, v5_00" and append a random value between 0 and 99. 
The result will be cut at length 5, you will end up having only minor version 00 for v5 and minor version 00 to 09 for v1.  

