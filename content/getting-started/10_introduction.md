---
title: "Introduction"
draft: false
weight: 10
---

**A typical modern application development**: 
* A modern architectures often build of several (up to hundreds) of connected services with huge amount of different technologies for communication and persistence
{{< figure src="/img/MicroserviceArchitecture.png" alt="Example" width="600px" >}}
* Typical system- and integration test are build by developers using the technology, which is the base for the service implementation. 
* In best case End-to-End testing (E2E) is done by dedicated testers using standard tools sticking to a few technologies. 
* Typical E2E Tests are one time shots: Start, Run, Destroy 



**Drawbacks of the current approach**:
* If E2E tests are in place, they mostly run on empty systems! The lack of data inside a testing environment often results in issues, which are identified on the production environment: Typical scenarios which are often not tested or tested with additional manual effort: 
  * Data dependencies due to data versioning within a system component or component environment overarching
  * Migration of data at runtime. In production this happens in parallel to the normal processes. Impacts on subsystems are often not sufficient considered. 
  * Environment behavior in case of massive data inside the systems. Does your system scale as expected? Can it handle all the data on all interfaces?
  * How does the system behave during the update of system components (e.g. during rolling releases).
* Which skills are necessary to implement new overarching test cases? Usually, you specialists know product XYZ. If your are new to a project you have to learn and understand the testing framework to maintain and extend it. 
* Who builds your test case? Usually its more or less a dedicated tester with a little understanding of the business workflow. The test case itself will be described by a business specialist without the knowledge of the testing framework. Why should the business specialist not describe the test case by himself? 
