---
layout: post
title:  "Micro-services: Anti-Patterns (Part 1)"
date:   2018-10-27 00:00:00 +0530
categories: micro-services
permalink: /:categories/anti-patterns-part-1.html
comments: true
---

### What are anti-patterns ?
Anti-Patterns are something, which developers choose as a design or implementation approach
which solves the issue at hand, but is not the optimal way of solving it. Since the developer
does not realize this at the moment, at a later stage, it becomes hard to change and developer has
to again re-design it.

### Microservices anti-patterns
#### **1. Too many Data Migrations**
If you are moving from monolithic to micro-services, you would have faced the daunting task
of migrating the data across different services. Lets say you figured out all finely grained
microservices and they have spanned into more than 30 micro-services (a really conservative count),
if you start all with it, it would a data migration nightmare.
##### **How to avoid it?**
Start with coarse grained micro-services, break your monolith into smaller number of micro-services.
 Relax, you can break them further later. What is more important is to ease the process of data migration. 
Less number of service mean less data to migrate.

#### **2. Functionality first, Data last**
Imagine, you have migrated to micro-services successfully. But over time, you notice that there
are few services which are becoming monolithic and require to be broken down further. The mistake
most developers do here is, divide the service's functionality and data together which makes the process
of breaking down service unstable, error-prone and tough for developers.
##### **How to avoid it?**
Break the service's functionality first, keep using the same monolithic service's database. Once you 
have successfully made the functionality separated into multiple service, then you may start extracting
out the database object one-by-one for the new services. It also avoids costly and repeated data migrations and makes it 
easier to adjust the service granularity when needed.

#### **3. Handling time outs**
Microservices rely a lot on asynchronous communication. Handling when to time out a request is really important
decision. Generally, most applications tend to calculate the maximum time taken (assuming, 2 seconds) to process a 
request under load and double it (4 seconds), thereby giving that extra buffer in the event if something takes a bit 
longer. But it causes it causes every request from service consumers to wait just to find out that service is not 
responsive. Four seconds is a long time to wait for an error.
##### **How to avoid it?**
Rather than relying on timeout values for your remote service calls, a better approach is to use something called the 
circuit breaker pattern. The circuit breaker continuously monitors the remote service, ensuring that it is alive and 
responsive. While the service remains responsive the breaker will be closed, allowing requests through. If the remote 
service suddenly becomes unresponsive, the circuit breaker opens, thus preventing requests from going through until the 
service once again becomes responsive. The significant advantage of the circuit breaker pattern over timeout values is 
that the service consumer knows right away that the service has become unresponsive rather than having to wait for the 
timeout value.

#### **4. "I was taught to share"**

While writing micro-service, it happens often that you would find code which is the same across all the services. You 
would think it is a good idea to extract it to a common place. Though it is absolutely fine to share code, but sometimes
developers go too far doing this and end-up making a mesh of dependent modules which make the whole system hard to 
change and test. It gets in the way when trying to migrate modules to a micro-services architecture. The more dependencies
you have between services, the harder it is to isolate service changes, making it difficult to separately test and 
deploy individual services. Sharing too much creates too many dependencies between services, resulting in brittle 
systems that are very difficult to test and deploy.

##### **How to avoid it?**
The first step is to carefully identify the shared pieces, keep the dependency count low and share the modules as 
stateless libraries (jars etc.).While creating such libraries, an important step is to identify when and how to create them.
In some cases, it is better to break the libraries in stable (rarely changes) and volatile (change too often), while in other
cases, developers might break it based on its responsibilities, like authentication lib, Date-time libs etc. Consistent 
versioning of such library is also a hygiene practice which is also an important part of software discipline.



We still have 4 more of such important anti-patterns and pitfalls to discuss about which would be published in the [second 
part]({{ site.baseurl }}{% post_url /micro-services/2018-10-27-anti-patterns-part-2 %}). Keep watching this space for updates.


Happy learning !!

