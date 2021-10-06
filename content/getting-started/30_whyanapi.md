---
title: "Why a Testing API?"
draft: false
weight: 30
---

A new project results in a new testing approach, depending on the used technology and the people working. 
Someone has experience with tool X, another one has already licenses for tool Y and another project member likes to approach to do everything from the scratch and new.
In best case you can find a common solution fitting all your requirements to develop a good test practice. 
In worst case you find a huge set of tests, which can not be executed or further improvement results in huge efforts.
Why not writing testcases in a common standardizes way, which is simple to extend and can be read and executed by arbitrary testing tools?
Don't forget, we are talking about describing **input** and **output** of an application.

That's it - lets define a common way of describe test cases, which can be executed by every one. 

## Why an API for Testing? 
The descriptive approach of test cases is finally just another interface, an interface between the tester (writing the test cases) and the test program executing the test cases.
How the tester writes the test case (via a simple editor or another UI, which helps him) is not important. 
Outcome is a description containing all necessary information for the test util to test the system under test.   



