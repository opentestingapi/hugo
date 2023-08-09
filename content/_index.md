---
title: 
description: open testing api

---

# OpenTestingAPI

The OpenTestingAPI is an open source initiative to harmonise the testing process over modern platforms. 
Goal is to establish a standard for testing definition which can easily be used with several tools, supporting backend and frontend development.

## Features

* **Simple**: Develop and apply testcases can be performed by nearby everyone
* **Flexible**: The concept is not restricted to technical platforms. It's explicitly designed for extension. 
* **Modern**: Testcases are designed in JSON, which is simple to read and supported by a wide range of technologies. This allows a simple integration into new and already existing tools.

## Deployment scenarios
* **Container**: Deploy as a permanently running container, upload tests and get them executed automatically (perfect for permanent data creation to simulate a "living" environment)
* **JUnit for one app**: Use Testcontainers to startup the OpenTestingApi container and all of the other components you need (databases,...) during your test-phase and execute the test cases as one-time-shots
* **JUnit for a landscape**: Use Testcontainers to startup OpenTestingApi container and run your tests against running application landscapes
* **Custom Pipeline integration**: Use bulk execution feature of our reference implementation to create your custom solution

Due to the compatibility with JUnit and the use of Testcontainers we can work with OpenTestingApi tests directly after the module test phase. Other test suites can be integrated via API - but the biggest advantage is the easy integration into existing pipelines as JUnit test.

## Changelog

* 1.4: support inject-inject chaining
* 1.3: Support multiple injection executions using 'executions' parameter (can be used to solve count requirements)
* 1.2: Intervall parameter for check execution
* 1.1: Checks support regex and JSON schema
* 1.0: Initial version

