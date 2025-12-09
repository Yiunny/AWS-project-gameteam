---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Building a Cost-Effective and Scalable Autonomous Driving Application with AWS RoboMaker

**MORAI** is a startup focused on autonomous driving technology, providing a simulation platform for validating and verifying autonomous driving systems.
Their simulator — **MORAI SIM** — is widely used across various domains such as autonomous vehicles, ADAS, automotive, robotics, and aerospace.
Companies across different industries, from autonomous vehicles, urban air mobility, robotics, to logistics, use **MORAI SIM** to test large volumes of complex scenarios, helping reduce costs, shorten time-to-market, and minimize regulatory risks.
In the public sector, government agencies and educational institutions — such as the Korea Transportation Safety Authority and multiple universities — also use **MORAI SIM** for training and regulatory compliance.
**MORAI SIM** enables virtual testing for autonomous vehicles by creating highly accurate simulation environments, including sensor models, vehicle models, and realistic scenario simulations.
The simulator can reproduce real-world scenarios using a high-performance rendering engine or by importing real vehicle log data.
In this article, we share how **MORAI**, an AWS customer, leveraged AWS RoboMaker to help developers run and scale autonomous driving simulations without managing infrastructure on their own.
By building a serverless architecture that integrates AWS RoboMaker with other AWS services, **MORAI** created a cost-efficient and scalable testing platform for large-scale autonomous driving validation.

---

## Challenges in Autonomous Vehicle Development

There is a significant difference between creating an autonomous driving demo and developing a commercial-grade product — real products require solid evidence that the system is sufficiently safe.
However, real-world testing of autonomous vehicles is extremely expensive and complex.
Whenever any part of the system changes, rerunning all tests becomes nearly impossible, making the development process slow and costly.
**Autonomous vehicle development** is one of the most resource-intensive challenges in the automotive industry.
To speed up development, **MORAI** needed a solution that would allow rapid building, testing, and managing of robotics applications.
Additionally, managing versioning and orchestrating hundreds of simulation scenarios across diverse computing resources was a major challenge.
Because MORAI serves many customers with varying testing needs, they experience sudden spikes in resource usage.
Therefore, they needed a more cost-efficient and flexible way to scale resources dynamically based on demand.

## Building a Cost-Effective and Scalable Autonomous Driving Application

Thanks to the AWS RoboMaker Batch API, **MORAI** automated the orchestration and management of simulation tasks.
AWS RoboMaker allows developers to focus on building the autonomous driving testing platform, including scenario-based testing and automated HD map generation.
As a result, developers at **MORAI** reduced software deployment time by up to **4 weeks**.
![Morai Stimulation Camera](/images/MoraiStimulation.png)
> *Figure 1. MORAI’s autonomous driving simulator modeling different lane-change scenarios.*

---

With **MORAI SIM Cloud**, autonomous vehicle developers can test scheduling, routing, and navigation algorithms at unprecedented scale.
The tool allows users to recreate and customize scenarios through a natural description language and an intuitive graphical interface to modify conditions such as weather, vehicles, or obstacles.
Developers can also select and configure various sensor models — such as cameras, LiDAR, and Radar — depending on test requirements.
Additionally, **MORAI SIM** provides a visual environment for configuring, calibrating, and monitoring vehicle dynamics.
This data-driven simulation approach provides value at every stage of autonomous vehicle development — from project initiation, testing, to final acceptance.
The architecture of **MORAI SIM** is designed to be a high-accuracy, large-scale, multi-sensor simulation platform.
This flexible, multilayered system can easily scale to test and validate automation software in various industries.
It helps customers save hundreds of thousands of dollars annually by reducing testing and validation risks.

---

## Solution Architecture

The **MORAI** team uses AWS serverless services to reduce infrastructure management and server costs.
Because usage prediction is extremely difficult in complex simulation environments, MORAI chose to build a flexible and cost-optimized serverless architecture.
The figure below shows the backend architecture of **MORAI SIM**, built using:
- AWS Lambda
- Amazon API Gateway
- Amazon Simple Queue Service (Amazon SQS)
- Amazon DynamoDB

These services automatically scale with demand, requiring no manual server management.
![Morai's Structure](/images/MoraiStructure.png)
> *Figure 2. MORAI’s simulation architecture using the AWS RoboMaker API.*

---

## Workflow

1. Users submit simulation requests (including algorithms and configurations). **MORAI SIM** receives the request and sends it to API Gateway.
2. AWS Lambda validates and parses the request. If valid, the simulation metadata is converted into JSON and sent to an SQS queue.  
   A typical request may contain **50–100 simulation tasks**.
3. To enable parallel execution, Lambda splits the large task into multiple smaller batches and sends them to a second SQS queue.
4. Another Lambda function retrieves tasks from this queue and triggers AWS Step Functions to start simulations when AWS RoboMaker is ready.
5. The state machine includes two Lambda functions:
   - The first Lambda creates the robot and simulation applications and launches the RoboMaker job.
   - The second Lambda monitors simulation status and logs results.
6. After completion, results and logs are stored in Amazon DynamoDB, and a completion notification is sent to the user.

---

## Conclusion

**MORAI** has discovered a scalable and cost-efficient approach to running autonomous driving and robotics simulations on the AWS Cloud.
By leveraging AWS RoboMaker and other serverless AWS services, MORAI significantly accelerated development, reduced operational costs, and enabled large-scale simulation without managing underlying infrastructure.

