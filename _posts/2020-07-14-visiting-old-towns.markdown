---
layout: post
title:  "Visiting old towns"
date:   2020-07-14 00:00:00 +0530
categories: java
author: "abhishek shah"
tags: java
---

## Visiting old towns

With so many things happening around this year we tend to lose ourselves in the middle of all this. The good question to ask is where are we heading? Can we handle the good, the bad all at once?

Being a Java application developer for five years working with a multinational company is turning very monotonous for me. I decided to work as a freelancer. I came across [Toptal](https://www.toptal.com), and immediately saw an opportunity that can help me in this transformation. 

But before we make this transition, let us revisit some of the projects I worked on, which can define my professional experience. I will also list the roles I played and the technical stack I used to complete my job.

The first project that comes in my list is a self-service portal I made with my teammates for a Swiss bank back in 2018. I will briefly describe the project and then spill out why it makes it to the top of the list. AppDynamics is a monitoring platform by CISCO. It can monitor applications made on different platforms such as Java, Dot Net, and NodeJs. The bank has over thousands of applications running critical business services that can never afford downtime. Hence AppDynamics was procured as an answer to IT wide application monitoring. A support team was created to help the different product owners onboard their applications to AppDynamics monitoring. But the support team was bombarded with gazillions of tickets, to help the product owners or their delegates onboard their product to AppDynamics monitoring. Each onboarding process involved the following tasks :
* An Excel sheet was maintained to track the license and generate billing reports.
* Erroneous configuration due to the manual process.
* Manually create Service Now tickets to get approvals to deploy the agents on the application servers.
* Manually deploy the correct versions of the monitoring agent on the application server.
* Upgrades/ Uninstallation of the agents.
These challenges opened the flood gates for the support team. Since all the tasks are exhaustive, and it invited unwanted heavy operational costs plus constant demand for more support.

So, on a fine afternoon, I went for a 3 pm casual coffee with my client. While sipping our coffee we talked about the challenges and the extra hours the support team has to slog over to help the product owners. We decided to automate everything and brewed a simple sketch to automate all the tasks for onboarding applications to AppDynamics.

After 8 months and countless coffee breaks, our solution was ready. Written in [Java EE 7](https://www.toptal.com/java-ee), with Hibernate as our choice of ORM tool and Websphere application server, we deployed it on Red Hat, it worked like a charm. Let me tell you about the things it could automate :
* Automatically populates all the server details.
* Prepopulates correct names for application based on internal naming conventions.
* Fetches the database details for database monitoring.
* Deploy the agent jar via one-click functions.
* Automatically Create Snow tickets to seek approvals to deploy the agent in production boxes. 
* Collect and aggregate license usage. 
* Equally, distribute the load on the AppDynamics controller in a Round-Robin fashion.
With all said about the product, let me walk you through my journey. Apart from being just a "developer", I got a chance to work with the different stakeholders to understand their concerns, I designed public APIs to integrate with other tools, quick prototyping, and countless demos. I also got a chance to improve my functional testing skills. In Agile based development, if something does not feel right then I would say trust your instincts. There is almost no wrong or right, just a dance between better and the best. We achieved the best with continuous improvement. This project transformed me from just a backend Java developer to something more. It defined me who I am today and what my client thinks of me.

A running solution which automated the mundane tasks and reduced operational costs by one-third gave food for thought. We started looking at other areas where we can automate the other services. Improve business performances and use more open-source Java libraries and tools. There is now a huge shift in the IT landscape of the bank, with almost every transformation, they are shifting to more and more open source. I also worked in another such engagement where the application previously in Java EE using Weblogic was shifted to [Spring Boot](https://www.toptal.com/spring-boot). 

[Java](https://www.toptal.com/java) community is transforming very fast with new releases coming quickly. Banks who were previously understood for their inconvenient archaic IT is now seen pacing towards open-source platforms. 

I hoped you liked my story. If you have such a story, please share it with me. I would love to hear from you guys.  Here are some facts about the project I discussed above:
I was only two years in the industry when I started working on the AppDynamics self-service portal.
The projects earned us around $400k, we were three people working on it, with one tech lead, one backend developer(me) and one frontend engineer.
The project reduced the operational cost by two-third or even less.
Zero loss in revenue generation, after utilizing the license reporting feature.
Opened more opportunities for transforming legacy applications.
That's it for today. In my next blog, I will write about how we managed to bring 70 thousand computers under decentralized monitoring. Till then, see you, stay safe. 
