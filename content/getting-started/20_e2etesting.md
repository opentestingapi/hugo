---
title: "End-To-End Testing"
draft: false
weight: 20
---

## Testing 
End-to-End (E2E) testing describes an approach which tests a system from its beginning to its end.
Each software system has at least one mission. The system is taking **input** data, **processes** this data and generates some **output** data. It's as simple - really. A system without these simple steps is a system without a mission - it's a useless system. 

Processing can be quite simple, take data from a place and put it to another one.

Considering this simple assumption, what is a test of a system? A test checks if the system generates for a given input the defined output. That's it! If the system does not generate the output as planned, the test failed and the system did not fulfilled its mission.

## Why so complicated? Why calling it End-to-End test? 
We did not talk about complexity of the SUT (System under test). This can be a single component or a complex software environment, which consists of dozen or hundreds of services. 
Processing data and finally the output of the system depends on several aspects. Often, you have more than only one input interface. 
All these interactions changes the internal state of the system. 

{{< figure src="/img/SUT.png" alt="Example" width="500px" >}}

Classical tests like module tests, integration tests or system integration tests expect a well-defined internal state. Goal is to test the system in an ideal reproducible state. 
Cover as much as possible of your features by test cases - often this is mistaken by cover as much as possible of your code, ignore the use case itself. 
Is it possible to test all internal states of a complex system with all potential external inputs? No. Often it's also not even possible to cover all corner cases together with different system internal states.

That's were E2E testing comes in! E2E testing does not require a clean homogenous internal state. 
On the contrary, an unpredictable internal state of the system can help to identify not yet covered failures of the system. 
Goal is to implement a test scenario which is from business perspective as near as possible on the production environment. 
An E2E test environment is continuous running testing system, which tests as much of your system features as possible in a production near environment! 

In an ideal world, you can take production data and inject them into your E2E testing environment. This results in several challenges. 
Often it's not as easy to get production data into your testing environment, sometimes by technical reasons sometimes by organizational restrictions.
With real data in your system it's not as easy to define and validate the expected output. 

{{< figure src="/img/SUTcheck.png" alt="Example" width="500px" >}}


 

