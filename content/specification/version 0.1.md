---
date: 2021-10-11
linktitle: Version 0.1
menu:
  main:
    parent: Specification
title: Version 0.1
weight: 10
---

# OpenTestAPI Specification

## Disclaimer
Part of this content has been taken from the great work done by the folks at the
<a href="https://www.asyncapi.com/docs/getting-started">[AsyncAPI]</a>.
Initiative and the 
<a href="https://github.com/OAI/OpenAPI-Specification">[OpenAPI]</a>.


## Introduction

## Definitions

### Services

A SUT can have several interfaces to interact. 
The current state of the Open-Test-API supports the following service to interact with:

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
kafka | [Kafka Object]({{< ref "#kafka-object" >}}) | Apache Kafka is a framework implementation of a software bus using stream-processing.
cassandra | |
oracle | | 
rest | |

Services can be part as well as of Injects and Checks. 

## Specification
### Format
### File Structure
### Schema
#### OpenTestAPI Object

This is the root document object for the API specification. 
It combines resource listing and API declaration together into one document.

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
testapi | [Version String]({{< ref "#testapi-version-string" >}}) | **Required** Specifies the TestAPI version. Can be used by clients to interpret the version. 
id | [Identifier]({{< ref "#identifier" >}}) | Identifier of the test case, which will be delivered.   
description | String | The informal description of the test case.  
injections | [Injections Object]({{< ref "#injections-object" >}}) | The set of injection objects, part of this testcase.
checks | [Checks Object]({{< ref "#checks-object" >}}) | The set of check objects, part of this testcase.  

#### TestAPI Version String
The version string signifies the version of the TestAPI Specification that the document complies to. 
The format for this string must be major.minor.patch. 
The patch may be suffixed by a hyphen and extra alphanumeric characters.

A major.minor shall be used to designate the TestAPI Specification version, and will be considered compatible with the TestAPI Specification specified by that major.minor version. 
The patch version will not be considered by tooling, making no distinction between 1.0.0 and 1.0.1.

In subsequent versions of the TestAPI Specification, care will be given such that increments of the minor version should not interfere with operations of tooling developed to a lower minor version. 
Thus a hypothetical 1.1.0 specification should be usable with tooling designed for 1.0.0.

#### Identifier
This field represents a unique universal identifier.
The format is in general free, but it can be used to package test cases to testsuites.
Good practice is to follow the URI format, according to <a href="https://tools.ietf.org/html/rfc3986">RFC3986</a>.

**EXAMPLE**
```text
{
    "id": "myproject:myexampleapplication:E2E:test1" 
}
```
```text
{
    "id": "myproject:myexampleapplication:E2E:test1:kafka:inject:mymessage" 
}
```


#### Injections Object
The injects object is a map of [Injection Objects]({{< ref "#injection-object" >}})

#### Injection Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
injectid |  [Identifier]({{< ref "#identifier" >}}) | **Required** A unique Identifier, which allows to connect an inject with several checks.   
service |  [Services]({{< ref "#service" >}})  | **Required** the service (interfaces), which will be used to inject the testing data. 
checks | [Check Objects]({{< ref "#check-object" >}}) | A set of `checkids`, describing the checks executed after the inject was triggered. 
cron | [Cron Triggers]({{< ref "#cron trigger" >}}) | The execution time of a job
sourcefile | [Filename]({{< ref "#filename" >}}) | The data source for the inject. The complete data of this file will be taken ans input for the SUT.

#### Checks Object
The checks object is a map of [Check Objects]({{< ref "#check-object" >}})

#### Check Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
checkid | [Identifier]({{< ref "#identifier" >}}) | **Required** A unique ID for the check.
service | [Services]({{< ref "#service" >}})  | **Required** the service (interfaces), which will be used to check the testing data.
expectedfile | [Filename]({{< ref "#filename" >}}) | The data source for the check object. The complete data of this file will be taken ans output validation for the SUT.
checktype | String | **Required** Check type defines, how to perform the output validation. Valid values are EQUALS, CONTAINS, ... . See [Check Type Definition]({{< ref "#check-type-definition" >}}) 
maxwaitime | [Time Duration Object]({{< ref "#time-duration-object" >}}) | **Required** This defines the maximum time a testing tools has to wait before it marks a check as failed. If the expected output arrives in the timeframe (inject-start, maxwaitime) the test will be reported as success.

#### Check Type Definition
The Open-Test-API supports the following types of validation:
CHECK    |  DESCRIPTION
---------|  -------------
EQUALS | The testing tool reports success if the output is equal to the expected output. 
CONTAINS | The testing tool reports success if the output contains the expected output. 

#### Cron Trigger

The Cron Trigger is a comma-separated list of cron jobs. The cron job definition follows the UNIX cronjob definition (<a href="https://en.wikipedia.org/wiki/Cron">[Cron Job]</a>)

The following examples will trigger the data inject every hour 5 and 15 minutes after the full hour.  

**EXAMPLE**
```text
{
    "cron" : [ "5 * * * * ?" , "15 * * * * ?"], 
}
```

#### Filename

The text content of file will be taken as the input for injects and/or checks. The filename can be fully-qualified/absolute path name or relative path to your data source. 
It's a good practice to have the pathname relative to your testcase description, e.g. all data sourcefiles for a testcase are inside a directory together with the testcase.    

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
#### Time Duration Object

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




#### Kafka Object
