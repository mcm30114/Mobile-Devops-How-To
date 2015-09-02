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

In an enterprise environment, there are many layers to the applications development and production process. Given the relationship of a mobile app to the back end systems that it requires data from, there may be a co-dependency on creating a middleware layer that acts as the access proxy for the mobile application into the back end systems.  

There are a few architectural patterns that come to mind such as "Mobile Backend as a Service" (MBaas), commercially available "mobile middleware" packages, an exposed and managed API manager, or a simple set of defined web services that are exposed by a public facing website that also has defined connections to a back end system, either behind a corporate firewall, or in a defined "yellow zone" area. 


> __NOTE:__ In network security terms, a __"Green Zone"__ is an isolated, protected, and known environment.  Think of it as the internal enterprise network that is isolated from the external public internet. A __"Red Zone"__ is an area that is publicly accessible, as in the "public internet".  A __"Yellow Zone"__, sometimes referred to as a __DMZ__, is a controlled sub network segment that allows for file transfer between the RED and GREEN zones, without explicit or continuous connection between the RED and GREEN zones.  It is used for maintaining isolation between the RED and GREEN zones while providing some means for controlled data interchange.


All of these "middleware" patterns require a discipline of environment progression to handle the maturity levels of software to be tested against.  As code is produced from individual developers, the complete solution is then provisioned into an environment for validation and testing. This would include the need to produce the Mobile Applications and the Mobile Middleware per environment.  An example of the environments that would be required to test the complete "mobile solution" (client and server components) are as follows:

 __DEVELOPMENT (DEV)__ - The Development Environment is typically the first integration environment where successful initial builds within a sprint are deployed.  Developers should have the ability to "build on demand" to deploy into this environment. Once code has been validated as stable, it should be promoted to the next environment.  Also, code that is "promoted" from DEV should NEVER be recompiled for successive environments.  They should be CONFIGURABLE for each environment.
 
__Systems Integration Test (SIT)__ - Systems Integration Test is an environment of stabilty and depending on the process of the team, can be deployed into either at the end of a sprint, or in several iterations in the sprint.

__User Acceptance Testing (UAT)__ - User Acceptance Testing is an environment where the product is stable, and is being exposed to a larger audience for testing and validation.  Beta testing may occur here. 

__Staging__ and __Production__ Environments should be identical in size, capacity, performance, and availability. This allows for accurate performance testing as well as providing a peer environment for immediate upscaling in case of strong user demand.  It also provides a mechanism for publication of code to one environment ("Staging") while the existing version is running active on the other environment ("Production").  Once the N+1 version is proven as stable as ready for production, a cutover can be performed such that "Staging" transitions to "Production", the existing "Production" is brought off line and becomes the new "Staging" (See "Blue-Green" production techniques)


A high level diagram to help show a flow of what takes place once an application is in the process of being developed is shown in Figure 1 below.

![alt text][figure-1]

[figure-1]: https://raw.githubusercontent.com/mcm30114/Mobile-Devops-How-To/master/Images/DevOps%20Flow.png "Figure 1"

Figure 1

There are a few key members of this flow.  You will see them referred to as PO, PM, DEV, TEST, and OPS.  Different project engagement processes may call them out as different roles, but for this exercise, here are my roles and definitions for this diagram:



| Role | Description |
|------|-------------|
|Product Owner (PO)| Person responsible for the vision of the project. Responsible for directing which features are required in what order for the ongoing work efforts of the team. |
|Project Manager (PM)| Person responsible for tracking the performance and guiding the team members for completion of individual tasks as assigned during a project phase of execution (sprint)|
|Developers (DEV)| People responsible for writing sections of code that will produce the desired outcome|
|Test (TEST)| People responsible for evaluating the published content (apps) created by the developers and validating whether the produced application meets specifications|
|Operations (OPS)| People responsible for the health and operation of the infrastructure needed to produce the application.  May also include managing and operating the servers, middleware, and configuration of such for the development construction environment, and the deploy targets for the produced code|

Also in Figure 1 above, as shown in the "boxes" there are some general processes or collections of functionality.  These are described below:

| Index | Name | Description |
|-------|------|-------------|
| A | PO - Backlog Management | A process where the Product Owner (and team) makes a determination on what function is produced in what order, and in what relative time spans should this software become operational.  Usually involves a "big picture" understanding of the problem, the infrastructure, dependencies, readiness of the organization, etc. Also helps define effort that should be completed in a sprint (defined short time span)|
| B | PM - Sprint Management | A process where the Project Manager (and team) takes the sections of functionality that have agreed to be completed in this time span (sprint) and break the tasks down into sections of effort that can be completed by one developer in a defined time period (half a day, a day, multiple days).  <br><br>If the tasks are as of such magnitude that requires more than one person, or more than 2-3 days to complete, then the team does a best effort to either decompose the problem into smaller assignable work units, or re-evaluate the task and sprint dependencies for alternatives. |
| C | Bug Tracking / Work-list / Collaboration | A tool or set of tools that allow the team to communicate as to the progress of work against a set assignment of tasks. These tools are accessible to all members of the team (although some features in these platforms may be restricted per role) and are used to help communicate issues of assistance, clarification, level of completeness of tasks on a project. <br><br> Typical tools you may see here are: Jira, Wiki's (Confluence), Shared Drives, Chat spaces (Hipchat, Slack, Lync), Kanban like tools (Trello), or other vendor specific tooling in this area. <br><br> These tools become very critical as the team becomes more distributed geographically.  Some environments do not use these tools because everyone sits in the same room or floor of a building. Some environments where the Product Owner(s) are in one section of the country (Headquarters), The Project Managers and certain key Subject Matter Experts are in another, the Developers are in several time zones away, and the Operations staff may be closer located to the data centers where the environments are deployed into.  |
| D | Development Platform | Specific compilers, integrated development environments (IDE), software development toolkits (SDK) that are used to turn developer produced code into executable assets. <br><br>  Typically used on developer workstations (laptops, desktops, etc), however there is a need specifically in the BUILD process (below) where scriptable, command line versions of these tools can be used to produce and test the same assets from when they are checked into the SCM and built. |
| E | Source Code Management (SCM) | A common repository of the project in it's current state.  The project may have an organizational structure to it to help define what section or branch of code is in production, what branch is in active development, which branch(es) may be in process to either work on active features, or to do maintenance work on, etc. <br><br> This tool is also very critical by allowing a historical view into the progress of the project. So if one team member "checks in" bad code, the code check in can be reversed back to a state where the code was last known as good.<br><br>  You may see products like Subversion (SVN), GIT, PVCS, Mercurial, or other code repository tooling here. |
| F | Build | The tools and process needed to take source code as submitted to the SCM, compile, assemble, and run scripts against to produce application artifacts.  This is one component of the "Continuous Integration" (CI) tool and process. <br><br>  Certain tools that may be seen in this grouping are: JENKINS, Xcode Server, or other CI tools. |
| G | Artifacts / Configuration Management | Results of the build process are taken and stored in an intermediate location.  Can contain content, application components (applications, deployment packages, bundles, etc).  Version number sensitive.  |
| H | Deploy / Release Management | A set of tasks that are typically managed by the OPS team to define what was built, what gets deployed, and when the code will be promoted from environment to environment. Requires that versioned content is pulled from the artifact management tool and then deployed per automation process to the target deployment environments. |
| I | BLANK | (Intentionally Omitted) |
| J | Deployment Environments | As code is deployed and matured, it progresses from environment to environment.  <br><br> Optimally, the "code" is developed one time and deployed to the DEV environment.  Promoting it from environment to environment should be managed by configuration management tools (Chef, Puppet, Urban Code Deploy, or other scripting methods). |
| K | Provisioning / Environment Configuration Management | Tools and processes that the OPS team then uses to bring quality and consistency to the establishment of the deployment environments. Automation and repeatability is key here.  Tools such as "Chef", "Puppet", "Docker" may be seen in these sections. |


Just to be clear, these steps are meant to be a general representation of the flow involving the steps and processes of software production.  Not every organization follows these exact steps.  How an organization does software production can have many factors -- some being process maturity, previously existing culture and governance processes, influences from other parts of the company, exact platforms that are involved in the end running and operations of the software being developed, etc. 

However, based on my experience with client-server applications, with web development applications, with mobile applications -- this process flow can be compared at a high level to be the same general pattern no matter what process you follow.



## Mobile Workflow 

The mobile workflow in itself is a modification of the previously illustrated high level software process flow.

In some instances where the mobile app is the only asset being developed and deployed -- where it has no dependency on other back end systems to make it a complete and functioning application -- then the Mobile Application Development workflow can be much smaller and more self contained.

Where the Mobile Application is part of a larger eco-system -- where there are multiple back end systems of record (a Profile system, a payment system, a catalog system, a transactional system, etc), where there are multiple "channels" of interaction with the system(s) such as customers being able to reach a customer service person to verify a transaction that they performed on the web, or on a mobile device -- then the interaction of interdependencies on with other systems can make the act of Mobile Application Development and Deployment much more complicated.

Examples of this level of complication could be the need to be in alignment with constantly revising web services or exposed API's, alignment of code capabilities between Mobile and MBaaS code when a new feature is rolled out (version control is another related issue here), or a general environment change, as in the introduction of a new component for the composite solution. 

In an effort to keep it simple, refer to Figure 2 below:

![alt text][figure-2]

[figure-2]: https://raw.githubusercontent.com/mcm30114/Mobile-Devops-How-To/master/Images/DevOps%20Flow%20Mobile%20Apps.png "Figure 2"

Figure 2

In relation to Figure 1, Figure 2 includes the general functional areas of "Provisioning and Creation", "Configure / Build / Functional Test" and starts to change the process a little in the area of "Deploy / Integration Test / Publish".  It also introduces a new area that was not explicitly called out in Figure 1 --  an area of post-production support, maintenance, and monitoring.  Mobile can be a little more unique in this area, but it does inherit some of this general pattern of processes from Web Application Development and Deployment.

There are some uniqueness that need some clarification.  Keep in mind that these processes inherit, align with, and / or overlay those processes defined in the high level pattern.: 

### Provisioning and Creation


| Index |  Process / Activity  | Description |
|-------|----------------------|-------------|
| A | Platform Provisioning| These are activities  that an organization needs to perform so that they can procure a license, an identity, an agreement, and / or a signing certificate from a Platform Provider. Each Platform Provider -- Apple, Research In Motion, Google, Microsoft -- has their own process to sign up, procure, and join one or more different programs that may have an affect on how an organization can ultimately distribute their application properly to the right audiences |
| B | Application Development| These involve getting the tools, provisioning profiles, certificates, SDK's and other associated content needed to develop for one platform.  Again, as above, each platform is different, and different tools and content may be required to develop for each platform. |

### Configure, Build, Functional Test


| Index |  Process / Activity  | Description |
|-------|----------------------|-------------|
| C | SCM | This is a replication of the same process that was described in Figure 1. There is not a lot of differences in the management of the project assets in a mobile application, but be aware that there may be more assets to be managed given whatever is intended to be bundled in the application itself.  The SCM system will have to accommodate complex data types beyond "just text" that the application is ultimately written into.  |
| D | Build | As mentioned in the High Level pattern, depending on the platform chosen, this can be implemented on generic server configurations (EG: Android using standard Java and related build tools) or the platform provider can mandate the use of special hardware configurations (EG: Apple requires the use of Apple Hardware, Mac OS X, and Xcode to produce iOS binaries).<br><br> In the case of Apple, the hardware and base OS has the capability of running the tooling needed to implement the other components of the build, test, and automation that would allow it to build some other platform components (EG: you can use a Macintosh to build and deploy Android binaries.  You cannot use a "linux" or "windows" machine to build an iOS binary).|
| E | Test | A replication of the High Level pattern for testing in general.  Some testing tools and processes may be platform specific, depending on what is used and who the platform provider is. <br><br> Where Mobile testing can diverge from the general pattern is that when an application is produced, it is highly unlikely that it will be specified to run on only one platform.  Even in the case where one platform is specified, there are typically multiple devices, if not device families, that the application needs to be tested against.  EG: In the current Apple iOS product line, there are iPod Touches, iPhones (small, large), iPads (small, large) and there are permutations around what storage capacity, and connectivity (Cellular, Wifi, Bluetooth) that need to be considered during the testing process. <br><br> It is not uncommon that for One "application" (EG: a transportation reservation app), it is generated for two "Platforms" (Android and iOS), and that application needs to be tested not only against a family of hardware with a variance of configurations, but there may be a need to test against various operating system versions that the population of users may be using.  This kind of situation may be more prominent for a company like "Facebook", "Google", or "Delta Air Lines", however, depending on the specified scope of users and their potential devices, this activity can be very intensive to complete. <br><br>There are some specific tooling that could be used for testing applications across all of these device permutations -- Perfecto Mobile, Xamarin, and others have multiple device testing processes.    |

One area that is not explicitly called out in this section is the area of collaboration, work list management, bug tracking, etc.  It is expected that these same activities would be performed in the Mobile Software development process. Again, assume that this "pattern" inherits from the High Level pattern described previously. 

### Deploy, Integration Test, Publish


| Index |  Process / Activity  | Description |
|-------|----------------------|-------------|
| F | Deploy | Deployment is an area that can be complicated or "involved" in a mobile development environment. <br><br> If the application is only going to be use internal to an organization, there are methods and tools that are well defined to help distribute these applications.   Some platforms allow for an "over the air" (OTA) distribution process.  Because of the lack of control of distribution, most organizations opt for a "Mobile Device Management" (MDM) tool that will allow them to control which applications get distributed to which users (groups).  It also allows for the removal of these applications from a user's mobile device for if the device is lost, stolen, damaged -- or if the employee departs the company for one reason or another.<br><br> If the application is going to be distributed to users outside of the construct of the enterprise, then using public app stores (EG: iTunes, Google Play) are the suggested outlets.  <br><br>Depending on what the "store" allows, an application can be placed on the store, beta tested, and then marked for public consumption once it's been properly vetted.  Time for release may be dependent on the specific app store as well.  For example, Google does not put too many controls on when an application can be released to the public.  Apple on the other hand, requires that all apps pass through a review process prior to publication -- and this process can vary from 3 days to 3+ weeks. Plan accordingly.|
| G | Beta Test | A segment of the Deployment process.  Organizations can choose the degree of test rigor that they can do prior to release of the application to the larger audience.  It is not uncommon to do a "rolling release" of an application -- once the application has reached a "near final" testable state, an organization may choose to release the app to a small number of users.  <br><br>During this release time, application feedback and automated behavior reports (crash logs, etc) will come back to be fixed and addressed. The next release may involve a larger audience, and the process repeats until there is a general consensus of confidence that the application -- and the infrastructure behind the application -- is ready for public release. <br><br> As noted above, certain platforms enable beta testing in different ways.  Plan accordingly.  |
| H | Internal / Enterprise | Used for when an organization wants to roll out an application that will only be used by internal users. OTA or MDMs are typically used here.  <br><br> If an organization does use this method of application distribution, and they are in compliance with the platform vendors terms and agreements of distribution, there is no constraints put on when, how often, or to whom (in your organization) gets the application. |
| I | BLANK | (Intentionally Omitted) |
| J | Public App Store | Used for "outside the organization" distribution.  <br><br> Different stores have different rules, timelines, and processes necessary to distribute the application to the correct audience. <br><br> Other issues that the public app stores have to consider is the payment model of the application, the distribution of "promotional codes" for free downloads to certain identified customers, the implementation of a "Business to Business" (B2B) store -- think of an "insurance agent" model of application distribution, or "Volume Purchase Plan" (VPP) store -- for when a large organization wants to centrally purchase a collection of licenses for your application at one time.   |

As an additional note, it is not uncommon for an organization to deliver one version of an application to internal users via an MDM or other controlled distribution process, and another version into the public app store to external users.  The management of the exact versions and distribution endpoints is an overhead that needs to be taken into consideration if this approach is chosen.


### Maintain, Monitor, Support


| Index |  Process / Activity  | Description |
|-------|----------------------|-------------|
| K | App Store Maintenance | As an application is prepared for publication, there are typically other assets that need to be prepared for proper store presentation.  Images, screen shots, narrative text (per version), change notes, icons, etc need to be prepared and submitted to the store alongside of the actual application package. These are loaded into the "store back end portal" that your developer account allows access into for as well as access to other back end reports. <br><br> Once an application is launched in any store, MDM or Public App Store, there is going to be a need to watch the performance of the application in the store and in the hands of the end users.  Most stores provide a "store portal" where metrics of download over time, feedback from customers, and other related "application consumption" data is collected. <br><br> Also, it is the accepted and expected process of a vibrant app to be rapidly updated.  The stores also help manage application distribution and version management, as long as the application development process has taken these issues of versioning control, etc into account.  |
| L | Post Launch Analytics | App stores only give a small amount of information as far as the lifecycle of an application is concerned.  You get application load metrics, and possibly some application crash logs. <br><br> To get a fuller "in the day of the life of your app" kind of metrics, you need to implement an analytics mechanism.  <br><br> Some tools like this are -- CRASHLYTICS, FLURRY, and in some cases Google Analytics can be used. |
| M | Marketing | Management of promotion of your application post launch. |
| N | Iteration Planning | Taking in the feedback of the users, crash logs, operational analytics and comparing them against KPI's and original requirements, giving a collection of content that can be used in consideration of what the next iteration of the application should include (or not).|
| O | BLANK | (Intentionally Omitted) |
| P | Support | As with any published application, support needs to be included and considered. |


Now that you have a good orientation on what the process looks like, lets get into some details -- where to look for more information, lessons learned, suggestions on processes, etc.


## Mobile Development Process Specifics

### DevOps in context of Mobile application development
"DevOps" is quickly becoming an *overloaded* term. Some folks refer to it as a process, some folks refer to it as a specific implementation of a set of products to enable a workflow. I'd like to keep any sense of "DevOps" at the level where it is an intention to move the product to completion in an automated and consistent manner. 

The intent of this document is to help show the DevOps processes needed to take an application from when it is checked into Source control from a developer's workstation through the process where it can be deployed onto a user's device. 

 

### Out of Scope
For this revision of the document, the following areas will not be covered:

- MBaaS setup, configuration, or related applications development

- Any project related activities that led up to the "build and deploy" phases of the project.  Issues of design, project engagement best practices, discovery processes, requirements management are presumed to have been handled prior. Issues of post application management will be covered, but not in any particular depth.

- The evangelization of one style of  mobile development approach over another (Mobile Web, Hybrid, Native).  It is expected that the output of the processes that are being defined here will produce an "Application Binary" that can be deployed on a mobile device, traditionally using a "store" paradigm for the ultimate endpoint distribution mechanism.

## Preparation

### Developer Programs
#### Android
The core android developer site can be found here --> https://developer.android.com/index.html

Training for the Android platform to produce your first app can be found here --> https://developer.android.com/training/basics/firstapp/index.html

To develop and deploy an application onto an Android device, you will need a developer credential (Signing Certificate).  Signing credentials are typically assigned to the DEVELOPER (person).  

Apps can be self-signed, but that works to the point where you need to test on your own devices. The general security model of the Android OS frowns upon the use and general distribution of self signed certificates. 

In the case of an enterprise, it is probably best to get a "key user" as a developer, or a developer persona, and then use that identity for the developer certifications and signing process.  This certificate can be used during the automated compile process of the build server.

#### iOS
The core website for Apple developers can be found here --> https://developer.apple.com

##### Standard Developer Program
By joining the general developer program, you have the ability to develop, sign, and submit an application to the Apple iTunes app store.  The general developer program runs about $99 (US) per year, and includes access to developer training content, videos from past "World Wide Developer Conference" (WWDC) proceedings. Starting in 2015, this one developer program covers the development of both the iOS and Mac OS X development process. 

Applications developed using the general developer program can be tested by using TESTFLIGHT to a controlled list of users, or by deploying onto a registered device (up to 100 devices per year, refreshed at the end of the year).  So testing and deploying an application in an iOS environment is a very controlled process. 

Developer Certificates are issued at the "developer account" level.  Many developers can be on the same team, so certificate management, depending on the structure of the development team, can be a challenge.

Applications are signed twice before they can be loaded on an iOS device from the Apple iTunes Store.  The first signing is the developer themselves.  Then once the application is delivered to, reviewed by, and approved by the Apple iTunes Store team, then Apple's store signs the app. After the final signing from the Apple "Certificate Authority" stamp of approval, then the application can legally be installed on a device, validate that the application can run (certificate checks). 

> __Note:__ In this case, Apple's Store certificate is considered to be the "top level Certificate authority" in the signing process.  This allows Apple to pull or expire an application in the case where either the application is deemed to be dangerous, malicious, or not in alignment with the terms and conditions of the developer agreement. Application certificate withdrawls can happen at the individual application level, or at the organization level. 

##### Enterprise Developer Program
Additionally, Apple has an ENTERPRISE DEVELOPER program that allows for enterprises (has a "DUNS number") to develop for and deploy internal to the organization without the involvement of Apple in the final signing process. The Enterprise Developer program is a separate program at $299 (US) renewable per year.

The intention of this developer program is to allow an organization to develop their own applications, and for them to be the official "top level certificate authority" in the signing process. This allows the application to be distributed internally, which can be done using "over the air" delivery techniques, or via a more preferred method employing a Mobile Device Management (MDM) system for application delivery control. 

As long as the device that the application is being deployed onto belongs to or is under the control of the Enterprise, for the purpose of their internal business operations only, then this should be compliant to the terms and agreements of the program. 

The issue of risk here, and what Apple wants to avoid, is the ability to distribute at will, outside of the enterprise and in competition with Apple, any applications that can be signed and installed as trusted. 

Some interesting conditions of note:

- The person who signs the agreement for the Enterprise Developer Program must be at a "legal enough" level for official contract signing, as well as the one who will be held responsible (read: taken to court) in the case where the terms and conditions of the agreement are violated. 

- Because the application that is signed with the enterprise certificate can be deployed anywhere, this process is typically used for the deployment and testing of the application prior to its publication to the intended internal audience.  Rather than "chew up" 100 testing slots, you can distribute at will (in a controlled manner) to any testing device in an organization.  It is highly suggested to use an MDM solution in place to control who has what version, to be able to revoke (kill) a "beta" application as needed, etc. 


####  Training

There are ample training materials and locations around to get trained.  There are companies like [The Big Nerd Ranch](https://www.bignerdranch.com) (BNR) that specialize in writing books about the development of the mobile platforms, teaching classes, and doing consulting in this general area. BNR covers topics of iOS, Mac OS X, Android, and Ruby development. 

For iOS, Apple has the iTunes U, which is a service for learning by videos.  There is a class called CS-193p from Stanford University that tends to get updated every Fall-Winter time frame to cover the applications development process.

For Android, there are many different online courses that can be taken from groups like Udemy, Lynda.com, Code School, etc. 

#### Hardware

There are a few areas of hardware that need to be in place to do proper development, testing, and deployment of mobile applications

##### Developer Workstations

Apple requires the use of Xcode as a development environment to produce iOS and Macintosh applications.  This requires some form of supported Apple hardware to develop on, as Xcode is only available from either the Apple App Store (for the Mac) or from the Apple Developer program website (which produces a disk image (.dmg) of a file that can only run on a Mac).  Loading and configuring XCode will also load in documentation and the required SDK's for iOS development. 

Supported workstations are: Apple iMac, Mac Mini, MacBook Air / Pro, or the Mac Pro workstation. Ideally, these should have retina displays, 8GB of Ram or larger, and an Intel i5 or i7.  Hard disk space should be no smaller than 128GB.

Android development workstation needs are not as prescriptive as Apple's development guidelines.  

For Android development, an 64bit x86 architecture machine (AMD, INTEL) should be fine for development, as the majority of the build tool chain involves Java tooling, open source tools, and the Android SDK. 

##### Build Servers

As with the development workstations, specific "classes" of hardware may be required.

For Android builds, any Linux, x86, 64bit based VM image or hardware based server should suffice.   Network access for administration of build tools and other resource access is required. 

For iOS builds, Apple hardware is required. Typical configurations are datacenter based Mac Mini's, or Mac Pro's.  I have seen, for "bootstrap" / short term needs, that a build machine may be a stand alone Macbook or iMac that is dedicated on the internal network for these tasks.  However, a datacenter managed server is the most optimal hardware deployment configuration desired. 

> __NOTE:__ It is not uncommon for an organization to use a dedicated bank of Mac Mini's as the "build slaves" for a Continuous Integration environment supporting the builds for iOS, for Macs, for Android, for Java general tools, etc.  Keep in mind, a Mac Mini is actually a great *Nix based machine that can run many of the tools that are required to build software. 

### Collaboration, Documentation, Source Code Management

#### Source Code Management

#### Collaboration Tooling



## The Build Environment

### Machine Provisioning 

#### iOS Development

#### Android Development

### Attached Test Devices

### Remote Testing



## The Build Process

### Versioning

### Release Notes

### Key Tools to have



## The Deploy Environment

### Internal Team: Beta Testing

### Consumer: App Store (iOS, Android)

#### iOS App Store

#### Android

##### Google Play

##### Other Android Stores

### Enterprise

#### Internal Team: Beta Testing

#### Internal 

## Post Launch Activities

### Analytics

### Store Management / Revision Management


## Key Lessons Learned

### Team access to resources

### Remote Access

### Testing Devices

### System Accounts

### Access Control Provisioning

### Developer Program Account Management

#### As a member on a team

#### As the principal owner of the Development Account

### MBaaS Systems Upgrades

## Anti-Patterns

### Overly Distributed Development teams
- split SCMs



## Odds and Ends

### Key Event Dates

#### iOS


#### Android










