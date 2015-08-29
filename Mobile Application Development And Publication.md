# Mobile Application Development and Production

Michael C Martin  
August 27 2015


## Overview
This doc will help define the process of what it typically takes to move a Mobile Application from the point where it is in some level of active development to the point where it can be loaded on a person's mobile device.

Also, this document is not meant to be an exact implementation guide. It is meant to be a general overview and explanation of the processes that I've seen to be successful in my projects that I have worked on.  There will be certain areas where I will point you to a specific reference point, but in general, think of this as a "cookbook" which was cobbled together as a result of collecting a series of recipes together that seemed to work at the time. 


 It has been my experience that while no two projects are exactly alike, there are certain patterns that are in place that have re-appeared consistently.  Some of these are platform (EG: Android, iOS, Windows) driven because of legal and technical compliance issues. Some of these are process driven -- such as the need to implement build tools, collaboration tools, and production tools to help speed and provide automation quality into the application production process.

Some assumptions:

1) The process of defining the scope and actions of the application are complete. We will not be discussing best practices around project formulation or team assembly.
  
2) The design docs of the application are complete.  All assets are final enough that developers and project leaders can draw a clear conclusion about what needs to be done.

3) Adhering to one project execution process over another is not overly important in this context.  However, because of the scale and interdependency of the solution for which the Mobile application was a component of, I may make reference to "water-fall-ish" processes intermixed with "Agile-ish" processes.

4) Tools and references will be referred to from time to time, but I am not advocating the use of one tool over another.  I am more concerned with the process and flow than I am the specific tools. 

5) This doc is meant to cover Mobile Application Development and will include references to Apple's iOS, Google's Android, and Microsoft's Windows platforms. 


## The High Level Overview of (one take of) the Applications Development Process 

Application development can be an individual sport or a team sport. An exceptionally talented person can develop, deploy, and manage an application today as is evidenced by the number of applications available in publicly accessible Mobile app stores (iTunes, Google Play, etc).  The steps that an individual developer and a team of developer undertake to publish an application is generally the same steps.  It is a pattern that can be captured and adapted to any mobile application publication process.  We will get to what that pattern looks like later in this document. 

When the Mobile Application is a component of a bigger picture and requires the resources of multiple people and interaction with multiple systems, there usually are other components and dependencies in play such as network infrastructure, back end systems, integration methods and tools, governance processes, etc.  

All of these components require a collection of individual team members to direct, operate, and manage.  It is the bringing of the people, process, and technology together that can be a challenge in an organization and some of that challenge has to do with culture, importance and criticality of the mobile app to the overall existing operations of the organization, as well as the maturity of the organization in application development processes.

In an enterprise environment, there are many layers to the applications development and production process. A high level diagram to help show a flow of what takes place once an application is in the process of being developed is shown in Figure 1 below.

![alt text][figure-1]

[figure-1]: https://raw.githubusercontent.com/mcm30114/Mobile-Devops-How-To/master/DevOps%20Flow.png "Figure 1"

Figure 1

There are a few key members of this flow.  You will see them referred to as PO, PM, DEV, TEST, and OPS.  Different project engagement processes may call them out as different roles, but for this exercise, here are my roles and definitions for this diagram:



| Role | Description |
|------|-------------|
|Product Owner (PO)| Person responsible for the vision of the project. Responsible for directing which features are required in what order for the ongoing work efforts of the team. |
|Project Manager (PM)| Person responsible for tracking the performance and guiding the team members for completion of individual tasks as assigned during a project phase of execution (sprint)|
|Developers (DEV)| People responsible for writing sections of code that will produce the desired outcome|
|Test (TEST)| People responsible for evalutating the published content (apps) created by the developers and validating whether the produced application meets specifications|
|Operations (OPS)| People responsible for the health and operation of the infrastructure needed to produce the application.  May also include managing and operating the servers, middleware, and configuration of such for the development construction environment, and the deploy targets for the produced code|

Also in Figure 1 above, as shown in the "boxes" there are some general processes or collections of functionality.  These are described below:

| Index | Name | Description |
|-------|------|-------------|
| A | PO - Backlog Management | A process where the Product Owner (and team) makes a determination on what function is produced in what order, and in what relative time spans should this software become operational.  Usually involves a "big picture" understanding of the problem, the infrastructure, dependencies, readiness of the organization, etc. Also helps define effort that should be completed in a sprint (defined short time span)|
| B | PM - Sprint Management |  |
| C | Bug Tracking / Worklist / Collaboration |  |
| D | Development Platform |  |
| E | Source Code Management (SCM) |  |
| F | Build |  |
| G | Artifacts / Configuration Management |  |
| H | Deploy / Release Management |  |
| I | (Intentionally Omitted) |  |
| J | Deployment Environments | As code is deployed and matured, it progresses from environment to environment.  <br><br> __DEV__ - The Development Environment is typically the first integration environment where successful initial builds within a sprint are deployed.  Developers should have the ability to "build on demand" to deploy into this environment. Once code has been validated as stable, it should be promoted to the next environment.  Also, code that is "promoted" from DEV should NEVER be recompiled for successive environments.  They should be CONFIGURABLE for each environment.   <br><br> __SIT__ - Systems Integration Test is an environment of stabilty and depending on the process of the team, can be deployed into either at the end of a sprint, or in several iterations in the sprint. <br><br> __UAT__ - User Acceptance Testing is an environment where the product is stable, and is being exposed to a larger audience for testing and validation.  Beta testing may occur here. <br><br> __Staging__ and __Production__ Environments should be identitical in size, capacity, performance, and availability. This allows for accurate performance testing as well as providing a peer environment for immediate upscaling in case of strong user demand.  It also provides a mechanism for publication of code to one environment ("Staging") while the existing version is running active on the other environment ("Production").  Once the N+1 version is proven as stable as ready for production, a cutover can be performed such that "Staging" transitions to "Production", the existing "Production" is brought off line and becomes the new "Staging" (See "Blue-Green" production techniques) |
| K | Provisioning / Environment Configuration Management | Tools and processes that the OPS team then uses to bring quality and consistency to the establishment of the deployment environments. Automation and repeatability is key here.  Tools such as "Chef", "Puppet", "Docker" may be seen in these sections. |


## Mobile Workflow 

## 





