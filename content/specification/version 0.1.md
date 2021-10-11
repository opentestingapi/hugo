---
date: 2021-10-11
linktitle: Version 0.1
menu:
  main:
    parent: Specification
title: Version 0.1
weight: 10
---

# OpenTestingAPI Specification

## Disclaimer
Part of this content has been taken from the great work done by the folks at the AsyncAPI Initiative. 
Mainly because it's a great work and we want to keep as much compatibility as possible with the AsyncAPI Specification.


## Introduction

## Definitions

### Services
FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
kafka | |
oracle | |
cassandra | |
rest | |


### Injects

### Checks


## Specification
### Format
### File Structure
### Schema
#### OpenTestAPI Object

This is the root document object for the API specification. 
It combines resource listing and API declaration together into one document.

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
opentestapi | Version String| **Required**
id | Identifier |
description | Description Object |
injections | [Injections Object]({{< ref "#injections-object" >}}) |
checks | | 

#### Injections Object
The injects object is a map of [Injection Objects]({{< ref "#injection-object" >}})

#### Injection Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
injectid | |  
service | | **Required**
checks | |
trigger | |
sourcefile | |

#### Checks Object
The checks object is a map of [Check Objects]({{< ref "#check-object" >}})

#### Check Object

FIELD NAME   | TYPE          | DESCRIPTION
------------ | ------------- | -------------
checkid | |
service | | **Required**
expectedfile | | **Required**
expectedtype | | **Required**
maxwaitime | | **Required** 


