# Architecture topics

# basics of well architected-framework

well-architected framework - framework that help to tackle and prevent common cloud computing challenges
set of strategies and guidlines for designing and running workloads in the cloud  
it also provides a consistent approach to evaluate the architectures  
benefits of applying well-architected framework  
1) avoid common pitfalls
2) decrease a risk
3) make sure your application is well-architected

well-arcitectured framework (further WAF, see video from 'links' section) pillars  
* cost optimization
* operational excellence
* reliability
* performance efficiency
* security
* **AWS** (sustainability)
For each of those pillars we have 'disign principles'

To meporize easier use pattern - **C.O.R.P.S.**

### cost optimization
- avoid unnecessar costs
- using right type of resources
- cost efficient decisions

### operational excellence
running and monitoring systems to deliver business value and continuity
- perform operations as code
- make frequent small reversible changes
- refine operations procedures frequently (constant improvement)
- anticipate failure
  - identify possible failures before they occur
- learn from operational failures

   
### reliability
focus on the ability of a system to recover from disruptions, dynamically meed demand and mitigate disruptions
- automatically recover from failure
- test recovery procedures
- horizontally scale to increase aggregate workload availability
- stop guessing capacity
- manage change through automation
  

### performance efficiency
- use computing resources efficiently
- maintain the efficiency as changes occur
  
### security
- focus on protecting data, systems and assests
- condidentiality of the data
- identity and access management
- controls to detect security threats

### sustainability (AWS)
- minimizing environmental impact of running cloud workloads

### 


# Reliability

## patterns

### circuit breaker
Prevents continuous requests to a malfunctioning or unavailable dependency.  
Handle faults that might take a variable amount of time to recover from, when connecting to a remote service or resource.
it might be pointless for an application to continually retry an operation that is unlikely to succeed, and instead the application should quickly accept that the operation has failed and handle this failure accordingly
The **Retry** pattern enables an application to retry an operation in the expectation that it'll succeed.    
The **Circuit Breaker** pattern prevents an application from performing an operation that is likely to fail.  

A circuit breaker acts as a proxy for operations that might fail. The proxy should monitor the number of recent failures that have occurred, and use this information to decide whether to allow the operation to proceed, or simply return an exception immediately.

# links
What is well-architected framework - MS Azure (video, 35 mins) - https://www.youtube.com/watch?v=vTjasx3ahjM&t
What is well-architected framework - AWS (video, 10 mins) - https://www.youtube.com/watch?v=MpDJ6TCWKjk
Get started with java on Azure (workshop, 40 mins) - https://learn.microsoft.com/en-us/training/modules/containerize-deploy-java-app-aks/
Tim Warner github page: https://github.com/timothywarner https://github.com/timothywarner/az305




