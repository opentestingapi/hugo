---
date: 2021-10-25
linktitle: Reference Implementation
menu:
  main:
    parent: Tooling
title: Reference Implementation
weight: 50
---

## Technology Stack
Our main goal is to make the test solution as easy to use as possible. 
In addition, it should be possible to extend the existing solution according to our own needs. 
For this reason we have chosen the widely used technology Spring Boot and Java.

Repository:
<a href="https://github.com/opentestingapi/impl_java">https://github.com/opentestingapi/impl_java</a> 

There are 3 options for use:

#### Container (preferred usage)
You can use all of the preexisting adapters, no need to deal with source code itself.

<a href="https://github.com/opentestingapi/impl_java/pkgs/container/opentesting">https://github.com/opentestingapi/impl_java/pkgs/container/opentesting</a> 

#### Extend the always latest version in retrospect
You can use all of the preexisting adapters, implement custom ones and inject into the latest source code as an addon.

<a href="https://github.com/opentestingapi/impl_java_extension">https://github.com/opentestingapi/impl_java_extension</a> 

#### Fork the project
You can fork our project, but you need to take care of new versions and API updates on your own.

## Features

Available Adapters: <a href="https://github.com/opentestingapi/impl_java#adapters">https://github.com/opentestingapi/impl_java#adapters</a> 

In addition to the main functions defined in the API, the reference implementation provides some extensions:

#### Metrics and Grafana Dashboard
Expose metrics for Prometheus and Grafana, including Grafana Dashboard

{{< figure src="/img/grafana_1.png" alt="Example" width="600px" >}}

#### API including Swagger UI
Communication should be API based, so we offer a huge list with API endpoints including Swagger UI.

<a href="https://github.com/opentestingapi/impl_java#api-usage">https://github.com/opentestingapi/impl_java#api-usage</a>

* test case scheduling
* reporting (export)
* trigger injects manually
* password encryption and decryption
* filter / search log
* pause test case execution

#### Pipeline Integration
For a pipeline integration you need a defined start time for test cases and the possibility to check the results later.
API offers additional endpoints to realize this "one-time-shot" feature as bulk execution of defined injects.
All subsequent injects/checks of the triggered injects are taken into account and the result can be determined on the basis of the bulk ID. 
By the possibility to define checks as not mandatory, the behavior of the pipeline can be adapted accordingly.

<a href="https://github.com/opentestingapi/impl_java#pipeline-integration--bulk-execution">https://github.com/opentestingapi/impl_java#pipeline-integration--bulk-execution</a>

#### JUnit Integration
You can use Testcontainers to startup our ref implementation during test phase with JUnit.
Using this approach it is possible to write system and system integrations tests with OpenTestingAPI!

<a href="https://github.com/opentestingapi/impl_java_testcontainers">https://github.com/opentestingapi/impl_java_testcontainers</a>

BTW <a href="https://github.com/opentestingapi/impl_java/tree/master/src/test/java/org/opentesting/devenv">Here</a> you can find a cool example to startup a local development environment powered by Testcontainers.

#### Preventing the locking of credentials
We have introduced a cache to stop repeating execution of wrong credentials.
Locked credentials could be simply unlocked by re-uploading the test case.
It can be disabled using the feature toggle in the properties, this can be useful for tests during non-zero-downtime deployments.

#### Tracing
TraceID from Brave is stored for every check and can be exported, Zipkin configuration is available.

#### Templating
Templating is not really a feature of the reference implementation, but here you can find a way to do it easily with the Linux shell.
For example, Connect information of the different stages can be managed and reused externally.

<a href="https://github.com/opentestingapi/impl_java/tree/master/scripts/json_template">https://github.com/opentestingapi/impl_java/tree/master/scripts/json_template</a>
