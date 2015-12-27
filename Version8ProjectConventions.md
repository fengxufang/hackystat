To best support collaboration in the development of Hackystat Version 8 services, please observe the following conventions:

# 1.0 Google Project Setup Conventions #

## 1.1 Project Naming: ##
  * Use all lower case letters.
  * Name your project hackystat-

&lt;serviceflavor&gt;

-

&lt;servicetype&gt;

.  

&lt;serviceflavor&gt;

 is one of: sensor, sensorbase, analysis, or ui.  

&lt;servicetype&gt;

 should describe the specific service, for example, "eclipse", or "dailyprojectdata".

Examples: hackystat-sensor-eclipse, hackystat-ui-sensordataviewer, hackystat-analysis-dailyprojectdata.

Note that "hackystat-sensorbase-uh" is an exception to this naming convention, as the 

&lt;servicetype&gt;

 component indicates an organization (University of Hawaii) rather than the  service type.  This is because there really aren't different "types" of sensorbase services as there are for sensors, analyses, and UI services. (There can be different implementations of the sensorbase, but they should be pretty much functionally equivalent.)

## 1.2 Links (Administrator page): ##
  * Please define a link called "Hackystat Version 8" that points to http://code.google.com/p/hackystat/

## 1.3 Blogs (Administrator page): ##
  * Please provide a link to the Engineering Log associated with the developers of this service.

## 1.4 Groups (Administrator page): ##
  * Please associate your project with the following groups: hackystat-announce, hackystat-dev, hackystat-users, hackystat-svn, and hackystat-issues.

## 1.5 Activity notifications (Administrator page): ##
  * Please send SVN commits to: hackystat-svn@googlegroups.com
  * Please send Issue tracker changes to: hackystat-issues@googlegroups.com

# 2.0 Issue Tracking Conventions #

The goals for Issue tracking are:
  * Provide a complete and useful record of the changes made to the system during each release cycle.
  * Provide a record of the major defects encountered in the system in order to support improvements to development practices.

To achieve these goals, please observe the following conventions:

## 2.1 Include a link to an Issue with each SVN commit ##

  * A relatively low overhead way to ensure a "complete" record of changes made to the system is to always include a link to one or more Issues in your SVN commit log entries. If a relevent Issue does not exist related to your work, this will remind you to create one before finishing the commit.  An example of this approach is: http://groups.google.com/group/hackystat-svn/browse_thread/thread/ee54692c3b25169b

  * For very simple and short fixes followed by a commit, it is onerous to create a new Issue.  Each Project can include a "Quick Fix" issue that can be linked in to in the commit log to indicate the minor nature of the change. An example of a Quick Fix issue for the Version 8.0 release of the SensorBase is: http://code.google.com/p/hackystat-sensorbase-uh/issues/detail?id=3


## 2.2 Include a Milestone as a label ##

  * To facilitate the use of the Issue tracker as a Change Log, please define a "Milestone" for each release. For example, "Milestone-8.0" represents the 8.0 release.  Attach a Milestone as a label to each issue which reflects the release in which this Issue is scheduled to be addressed.  For example: http://code.google.com/p/hackystat-sensorbase-uh/issues/detail?id=2

  * You will occasionally want to create an Issue to document a new proposed feature, but not explicitly tie it to the next release.  A convention for this is to define a Milestone called "8.x".  This indicates that the Issue is scheduled to be addressed at some point in Version 8, but the actual version is not yet decided upon.


