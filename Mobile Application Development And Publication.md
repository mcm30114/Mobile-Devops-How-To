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

3) Project execution process is not overly important in this context.  However, because of the scale and interdpendency of the solution for which the Mobile applicaiton was a component of, I may make reference to "water-fall-ish" processes intermixed with "Agile-ish" processes.

4) Tools and references will be referred to from time to time, but I am not advocating the use of one tool over another.  I am more concerned with the process and flow than I am the specific tools.

5) This doc is meant to cover Mobile Application Development and will include references to Apple's iOS, Google's Android, and Microsoft's Windows platforms. 


## The High Level Overview of (one take of) the Applications Development Process 

Application development can be an individual sport or a team sport. When the Mobile Application is a component of a bigger picture, there usually are other components and dependencies in play such as network infrastucture, back end systems, integration methods and tools, etc.  All of these components require individual team members to direct, operate, and manage.  It is the bringing of the 
