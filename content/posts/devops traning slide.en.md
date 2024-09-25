+++
Categories = ["Technology"]
Description = "Devops training"
tags = ["devops"]
date = "2022-06-17T10:40:37+08:00"
menu = "architect"
title = "Devops traning"
toc = true
+++

# Devops #

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops). It aims to shorten the systems development life cycle and provide continuous delivery with high software quality.DevOps is complementary with Agile software development;

CI/CD is a best practice for devops and agile development. CI/CD bridges the gaps between development and operation activities and teams by enforcing automation in building, testing and deployment of applications. CI/CD services compile the incremental code changes made by developers, then link and package them into software deliverables. Automated tests verify the software functionality, and automated deployment services deliver them to end users. The aim is to increase early defect discovery, increase productivity, and provide faster release cycles. The process contrasts with traditional methods where a collection of software updates were integrated into one large batch before deploying the newer version. 

Modern-day DevOps practices involve:

- continuous development,
- continuous testing,
- continuous integration,
- continuous deployment, 
- continuous monitoring
![image](/images/Continious-integration.png)

Tools for differrent phase

![image](/images/DevOps-process.png)


 # Test Types  #

## Unit Test ##
- Function coverage
- Statement coverage
- Path coverage
  
![image](/images/path-coverage.jpg)


## Function Test ##

In functional testing, each function tested by giving the value, determining the output, and verifying the actual output with the expected value.

- Positive testing determines that your application works as expected. If an error is encountered during positive testing, the test fails.

- Negative testing ensures that your application can gracefully handle invalid input or unexpected user behavior

![image](/images/functional-testing-intro.png)
![image](/images/functional-testing-types.png)

## Regression Test ##

- Regression Testing is defined as a type of software testing to confirm that a recent program or code change has not adversely affected existing features
-  Functional testing is performed to ensure all functionalities of an application is working as expected, whereas regression testing is performed once a build is released to check the existing functionality.

## System Integration Test ##

- SIT with client real /client simulator environment

 
## User Acceptance Test ##
- User Acceptance Testing (UAT) is a type of testing performed by the end user or the client to verify/accept the software system before moving the software application to the production environment.

- 
## Performance Test ##

- Performance testing is a testing measure that evaluates the speed, responsiveness and stability of a computer, network, software program or device under a workload. Organizations will run performance tests in order to identify performance-related bottlenecks.



## Various Devops tools ##
 ![image](/images/DevOps-tools.jpg)


# Jenkins for Test Automation(Build Deploy Test) #

Jenkins is a popular CI orchestration tool. It provides numerous plugins for integration with multiple test automation tools and frameworks into the test pipeline. 

- Runs Automated Test Suites: Jenkins provides plugins for various test frameworks like Selenium, Cucumber, Appium, Robot framework, etc. These can be integrated into CI pipelines to run automated tests for every build
- Summarizes the results: Most plugins also summarize the test results and display them as an HTML page.
- Provides Trends: Jenkins keeps track of results and displays them as a trend graph. This offers a better view of how the tests have fared in the past.
- Display details on Test Failures: Test results are tabulated, and failures are logged with the test results.


 ![image](/images/jenkins.jfif)








