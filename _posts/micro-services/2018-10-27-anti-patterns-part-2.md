---
layout: post
title:  "Micro-services: Anti-Patterns (Part 2)"
date:   2018-10-27 00:00:00 +0530
categories: micro-services
permalink: /:categories/anti-patterns-part-2.html
comments: true
---

### Microservices anti-patterns(contd..)

In the [previous]({{ site.baseurl }}{% post_url /micro-services/2018-10-27-anti-patterns-part-1 %}) article, we discussed
few anti-patterns prevalent in micro-services. This article continues from there and discusses few more of them.

#### **5. Reach-in Reporting**

Since, in the world of micro-services, services and the data are contained with in a 
[bounded context](https://martinfowler.com/bliki/BoundedContext.html){:target="_blank"}, it makes reporting a really 
tough job. Developers have to write reporting service which talks to multiple services, gathers the data, processes it,
and renders to the client. But how does reporting service get the data? There are three major techniques of it:
 * ***Database pull model***: Here, the reporting service directly pulls the data from the databases of other micro-services, 
    processes it and renders. Downside being, services no longer own their data. Any service database schema change or 
    database refactoring must include reporting service modifications as well, breaking that important bounded 
    context between the service and the data.
 * ***HTTP pull model***: Here, instead of talking to database directly, it talks to other services via HTTP requests. Though 
    it does protect the bounded context, but is too slow for day-to-day complex reporting operations. Imagine 10 REST 
    calls going in and out to prepare data. What if one of them fails? (As a developer, you must be scared of this !!)
 * ***Batch pull model***: Here, reporting service maintains its own database, populates it via nightly batch jobs which pull 
    the data from other services' database directly. But, it has the same problem as database pull model. Bounded context 
    is broken.

#### **How to avoid it?**        
 * ***Asynchronous Event Pushing***: Each micro-service asynchronously sends its data updates (the data the 
    reporting service needs) as a separate event to reporting service, which then processes the data and updates the 
    reporting database. The event-based push model requires contract between each micro-service and the reporting service
    for the data it is asynchronously sending, but that contract is separate from the database schema owned by the service.
    Since it asynchronous, hence there are no performance challenges as well.    

#### **6. Grains of Sand Pitfall**

Perhaps the biggest and most obvious question every micro-services project has: How big should a service be? How small 
should it be? Choosing the right level of granularity for your services is critical to the success of any micro-services
effort. Service granularity can impact performance, robustness, reliability, change control, testability, and even 
deployment. This anti-pattern occurs when developers create services that are too fine-grained. Sometimes a class is thought
as a component which makes matters worse.

#### **How to avoid it?**     

The avoidance lies in detecting this anti-pattern. So how can we detect it?

There are three main areas which can help to determine service granularity.   
 * ***Service scope and function***: What does the service do? What are its operations? Documenting or verbally stating 
 the service scope and function is a great way to determine if the service is doing too much. Using words like "and" 
 and "in addition" is usually a good indicator that the service is probably doing too much. Write down the operations 
 performed you can spot mutually exclusive operations, it should be broken there.
 
 * ***Database Transactions***: Generally, a typical database transaction is expected to be 
 [ACID](https://en.wikipedia.org/wiki/ACID_(computer_science)){:target="_blank"} compliant. But in micro-services, we rely on BASE(basic 
 availability, soft state, and eventual consistency) transactions. Regardless, there will usually be times where you do 
 require an ACID transaction for certain business operations. If you constantly find yourself struggling to maintain BASE
 transaction to perform operation which are more like ACID transactions, chances are that you have made your services 
 too fine-grained.
 
 * ***Service Choreography***: Service choreography refers to the communication between services, also commonly referred
  to as inter-service communication. it decreases the overall performance of your application since each call to another
   service is a remote call which takes time. The other issue with too much service choreography is that it can impact 
   the overall reliability and robustness of your system. The more remote calls, you make for a single business request, 
   more are the chances are that one of those remote calls will fail or time out. If you find you have to communicate 
   with too many services to complete single business requests, then you’ve probably made your services too fine-grained.

#### **7. "Developer without a cause"**
"Programmers know the benefits of everything and the trade-offs of nothing." but a developer must understand both. If the
 services are too fine-grained, therefore, impacting performance and reliability, the developer takes the decision of 
 consolidating the fine-grained services into more coarse-grained services. But due to that, Deployment, change control 
 and testing becomes harder. 
 
 On the other side, if the services are too coarse-grained, testing and maintainability is harder and the developer 
 might think splitting it up in fine-grained services is better which may bring issues of performance and reliability.
 
#### **How to avoid it?** 
 
 Understanding the business drivers behind choosing micro-services is the key to avoiding this pitfall. Every developer 
 on the team should know the answer to each of the following questions:
 * Why are you doing micro-services?
 * What are the primary business drivers?
 * What architecture characteristics are most important?

If the primary reason of doing micro-services is to achieve better time to market via an effective deployment pipeline. 
It means deploy-ability of each service outweighs performance, reliability, and scalability. Hence fine-grained is a 
better choice. Similarly, if the reason for moving to micro-services is to increase the overall reliability and robustness of the 
application. Meaning, you would likely trade-off ease of testing and deployment for better reliability and robustness, 
therefore, favoring more coarse-grained services rather than finer-grained ones.

One old-school but effective technique to avoid this pitfall is to write the business drivers in big red letters on the 
top of the common team whiteboard. Whenever in future, there is a granularity decision to make, just read the big red 
letters and you will probably find your answer soon.

#### **8. Jump on the Bandwagon**
The jump on the bandwagon pitfall is all about embracing micro-services before analyzing your business needs, business 
drivers and overall organizational structure and technology environment. While the micro-services architecture is a very
 powerful and popular architecture style, it’s not suited for every application or environment.
 
#### **How to avoid it?** 

You can easily avoid this pitfall by first understanding the advantages and disadvantages of micro-services. 

* ***Advantages***:
    - ease of deployment
    - ease of testing
    - ease of release
    - modularity
    - scalability
* ***Disadvantages***
    - Development teams must be restructured and reorganized into more cross-functional teams so that small teams can 
    own the end-to-end technical aspects of the services. The traditional corporate development team model of user 
    interface teams, backend development teams, and database engineers/administrators simply doesn't work with a 
    micro-services architecture. 
    - Release process changes.
    - Maintaining performance is harder due to inter service communication.
    - Maintaining data Reliability is even harder.
    - DevOps is hard.

 After understanding the advantages and disadvantages of the micro‐services architecture style, you must then analyze 
 your business needs and goals to determine if micro-services is the right approach  for the problem you are trying to 
 solve. When determining whether micro-services is a fit, ask yourself the following questions:
 * What are my business and technical goals?
 * What am I trying to accomplish with microservices?
 * What are my current and foreseeable pain points?
 * What are the primary driving architecture characteristics for this application (e.g., performance, scalability, maintainability etc.)?
 
 Answering these questions can help you match up your business needs and goals with the advantages and disadvantages of 
 micro-services to determine if it is truly the right fit for your situation.
 
 If you want to learn more about micro-services anti-patterns, you may read this [book](https://www.oreilly.com/library/view/microservices-antipatterns-and/9781492042716/){:target="_blank"}
 
 If you have any questions or feedback, please put your thoughts in the comments section below.
 
  Happy learning !!