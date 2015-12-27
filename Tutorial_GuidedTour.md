## 1.0 What is Hackystat? ##

Hackystat is a framework for collection, analysis, visualization, interpretation, annotation, and dissemination of software development process and product data.  This is an ambitious goal, and in order to address it, Hackystat is organized as a collection of loosely coupled software services that communicate using [REST architectural principles](http://en.wikipedia.org/wiki/Representational_State_Transfer).  The following diagram shows a small set of Hackystat services and how they communicate with each other:

![http://hackystat.googlecode.com/svn/wiki/architecture.gif](http://hackystat.googlecode.com/svn/wiki/architecture.gif)

Starting at the bottom, the blue boxes represent "sensors".  Sensors are small software plugins that typically collect data from the use of software development tools.  A Hackystat sensor could also collect data directly from developers, but since developers tend to be quite busy, we generally try to avoid requiring the developer to provide low-level data to the environment manually.

Sensors send their data to a repository called the SensorBase, represented here by a purple box.  In this drawing, there is only one SensorBase, but the Hackystat architecture is designed to make it simple to distribute data among many SensorBase instances. The SensorBase provides a repository for fundamental resources in Hackystat: sensor data, sensor data types, users, and projects.

Sensor data tends to be quite low-level and voluminous, and in most cases needs to be analyzed and abstracted in some way in order for it to be of use to developers.  The green boxes represent examples of Hackystat analysis services.  The DailyProjectData analysis provides a set of abstractions generated from all of the raw sensor data for a given day and project. The Telemetry analysis takes DailyProjectData analyses along with raw data from the SensorBase and produces trend-based information about software development. Boswell is an experimental analysis that supports awareness in distributed work groups as well as a kind of developer "biography".

Finally, the gold boxes represent user interface services, which provide information from the Framework to the user, and in some cases allow the user to feed information back into the Framework.  For example, the ProjectBrowser displays both data from the SensorBase and DailyProjectData analyses, and allows the user to create and modify Project definitions stored in the SensorBase.  Hackystat user interfaces need not be limited to web browsers, and we are exploring alternative user interface mechanisms such as ambient devices.

In keeping with REST architectural principles, all of the arrows in this
diagram represent communication among services using standard HTTP GET,
PUT, POST, and DELETE operations. One implication is that Hackystat is
quite platform and technology neutral. For example,  a sensor implemented using .NET
could collect raw data that is analyzed with a service implemented using
Java, and the results could be presented through a web application
implemented using Ruby on Rails.

## 2.0 Registering and sending data ##

To use Hackystat, you begin by registering for an account with a Hackystat SensorBase.  You can download and install your own local set of Hackystat services and then register with your local SensorBase, or you can register with the public Hackystat SensorBase  maintained by the [Collaborative Software Development Laboratory](http://csdl.ics.hawaii.edu) at the [University of Hawaii](http://www.hawaii.edu).

For example, to register with the public Hackystat SensorBase, you can go to the ProjectBrowser user interface home page at http://dasha.ics.hawaii.edu:9879/projectbrowser/:

![http://hackystat.googlecode.com/svn/wiki/register1.gif](http://hackystat.googlecode.com/svn/wiki/register1.gif)

I've typed in my email address, and after pressing the "Register" button, the system will send you an email with the password to your newly created account:

![http://hackystat.googlecode.com/svn/wiki/register4.gif](http://hackystat.googlecode.com/svn/wiki/register4.gif)

Now that you have an account, you can download and install sensors.  For example, the Emacs sensor collects data about the files you are editing.  Here is an image of my Emacs window at the moment when I was typing a version of this documentation.  The upper window shows the file I was editing, and the lower window, which is normally hidden, shows the Hackystat Emacs sensor at work in the background, recording information about what I am doing.

![http://hackystat.googlecode.com/svn/wiki/emacs.gif](http://hackystat.googlecode.com/svn/wiki/emacs.gif)

Sensors are available for many development tools. For a complete list, and instructions on how to install each of them, see the [Component Directory](ComponentDirectory.md).

## 3.0 Displaying Sensor Data ##

Once you have installed and configured your sensors, you can go back to working on your projects in the normal way.  The Hackystat sensors will unobtrusively monitor your development activities and send sensor data off to the SensorBase server with which you are registered.

To see what raw sensor data has been sent by the server to the Hackystat SensorBase, you can use the ProjectBrowser. Using the public Hackystat services as an example, you can go back to the public ProjectBrowser at http://dasha.ics.hawaii.edu:9879/projectbrowser/, and this time log in using your email address and the password sent to you by the system.  This will log you in to the ProjectBrowser and take you to its SensorData page:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.1.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.1.gif)

This page enables you to see what data your installed sensors have sent to the system for a given month.  Let's choose August, 2008 and the "Default" project, and here's what the system returns for me:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.2.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.2.gif)

The SensorData page creates a table with a row for each day in a month, with links for each tool that sent data and the type of sensor data that it sent.  For example, you can see that on Friday, August 1, 2008, my Subversion sensor sent 12 Commit sensor data instances, and my Emacs sensor sent 14 DevEvent sensor data instances.

When you click on a link, the ProjectBrowser pops up a modal window displaying all of the sensor data instances associated with that link.  For example, clicking on the "8:Ant" link pops up the following modal window:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.3.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.3.gif)

For more about sensor data display, see the [Sensor Data Collection Tutorial](Tutorial_SensorDataCollection.md).

## 4.0 Organizing your data using Projects ##

Most developers work on more than one project at a time, and many developers work as a team on one or more projects.  Hackystat provides "Projects" as a way to support both those work practices; a given Project filters which of your sensor data should be associated with that project, as well as aggregate your filtered data with the filtered sensor data from other users to support analyses of the overall team's behavior.

The ProjectBrowser provides an interface for viewing, creating, deleting, and modifying project definitions:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.4.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.4.gif)

This page lists the two  projects that I participate in.    The first project is called "Default", which is a Project that is automatically defined for all users and cannot be edited.  The Default project is specified to match all of the sensor data that you have sent, and only that sensor data.

The second project is called "Hackystat", and unlike the Default project which always has only a single participant, the Hackystat project has around a dozen participants.

This page illustrates some of the important components of a Hackystat Project:
  * The owner.  There can be only one.
  * Zero or more members, invitees, or spectators.
  * A start and end date for the project.
  * One or more "URI Patterns", which filter sensor data based upon their Resource field.

To learn more about Project definition and management, see the [Projects Tutorial](Tutorial_Projects.md).

## 5.0 Understanding your sensor data: Daily Project Data ##

Looking at the raw sensor data rarely allows you to gain insight into your software development practices and how to improve them.  Hackystat provides a variety of abstractions and analyses based upon the raw sensor data.  One very useful set of abstractions are referred to as "Daily Project Data".  These abstractions, as the name suggests, provide a perspective on process or product measures for a given project on a given day.   Generating a Daily Project Data analysis requires the system to merge together and/or filter raw sensor data from all developers associated with the project for the given day.

The ProjectBrowser application provides a page with a user interface to many of the Daily Project Data analyses. Here is an example screen shot showing information about the complexity of a Hackystat project called "simpletelemetry" on a single day:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.6.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.6.gif)

You can select multiple projects if you want to perform a comparative analysis of them for a single day.

## 6.0 Understanding your sensor data: Software Project Telemetry ##

The Daily Project Data analyses can be interesting and useful by themselves, but Hackystat builds on this analysis with a higher level abstraction called [Software Project Telemetry](http://csdl.ics.hawaii.edu/research/SoftwareProjectTelemetry).  Software Project Telemetry was developed by Qin Zhang for his Ph.D. thesis.

Software Project Telemetry is an approach to in-process software project management based upon the generation and analysis of process and product measures and their trends over time.    The ProjectBrowser application provides a high level interface to some of the features of Software Project Telemetry.  Here is an example of a Telemetry chart for a simulated project called "simpletelemetry":

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3.gif)

This telemetry interface allows you to select a "Chart", which defines a set of trend lines, or "streams" to display.  You must also specify one or more projects and a time interval for the telemetry.

This telemetry illustrates a hypothetical project that is "in trouble":  test coverage is decreasing, complexity and code issues are increasing, and test invocation frequency is low.

For more about telemetry, see the [Software Project Telemetry Tutorial](Tutorial_SoftwareProjectTelemetry.md).

## 7.0 Understanding your sensor data: software project portfolio analysis ##

Software Project Telemetry provides a way to visualize trends in your process and product data, and to see how several trends relate to each other at once.  However, there is an upper limit to the number of trend lines that can be displayed at once.  Beyond, say, 10 or 15 trend lines, telemetry data tends to be difficult to read and interpret.  This makes telemetry unsuited to organizations that might want to compare 20, 30, or more individual projects and their trends.

A second issue with telemetry is that the visualization is "value-neutral": it does not provide any indication of whether the trend direction (or trend data values) are good or bad.  However, in many organizations, it is possible to establish generally useful interpretations such as "coverage below 50% is bad, coverage above 95% is good", and "decreasing complexity is good, increasing complexity is bad".

To support visualization of large numbers of projects and their associated trends, Hackystat provides Software Project Portfolio Analysis (SPPA).   For illustrative purposes, here is a screen shot of the Portfolio page from the ProjectBrowser UI on three simulated projects:

![http://hackystat.googlecode.com/svn/wiki/projectportfolio.1.gif](http://hackystat.googlecode.com/svn/wiki/projectportfolio.1.gif)

Without even understanding the details, you should be able to tell that the first row of data associated with the project named "GoodProject" is "good", in the sense that this project's data is colored almost all green.  Similarly, you should be able to see that the second row of data associated with "TroubledProject" is "bad", in the sense that this project's data is colored almost all red.  Finally, the last row, for "UnstableProject", is mostly colored yellow, which means that it is neither good nor bad at the moment, but somewhere in between.

This is the first important feature of SPPA: an interpretation is being made of the data values.  Looking a little bit more closely, you can see that each column containing a data type (such as "Coverage", "Complexity", or "Coupling") contains two kinds of data per project: a [Sparkline](http://en.wikipedia.org/wiki/Sparkline) histogram showing the trend, and a numeric value indicating the most recent value. Each of these are colored independently: the histogram is colored to reflect whether the trend is good or not, while the numeric value is colored to reflect whether that specific value is good or not.

The second feature of SPPA is that not all data values are colored red, yellow, or green. In the example, Commit, Build, UnitTest, FileMetric, and DevTime data is displayed in white.  This indicates that not all Hackystat data has a simple interpretation: it is not clear, for example, what a "good" size for a system is, or that increasing size is necessarily a positive thing.  On the other hand, these data values might provide important contextual information: for example, the UnstableProject is an order of magnitude bigger than the other projects, which might influence the data values.

Finally, SPPA provides a kind of "information compression" not present in Telemetry.  Even this very simple example illustrates the equivalent of 30 telemetry streams (10 telemetry streams for each of 3 projects).  It is easy to see that this table could easily grow to 20 or 30 projects without any loss of usability, which corresponds to 200 to 300 telemetry streams.

There are a number of interesting details regarding SPPA, including how to configure the interpretation, and how to "drill down" to the telemetry associated with a particular Sparkline.  For these details, please see the [Software Project Portfolio Analysis tutorial](Tutorial_ProjectPortfolio.md).

## 8.0 Understanding your sensor data: previous Hackystat research ##

Over the years, the Hackystat Project has investigated a number of different approaches to analysis of sensor data.  Here are a few of these projects:

  1. [Priority-ranked Inspection](http://csdl.ics.hawaii.edu/research/PriorityRankedInspection), developed by Aaron Kagawa, which determines which modules are most in need of software inspection;
  1. [Zorro](http://csdl.ics.hawaii.edu/research/Zorro), developed by Hongbing Kou, which determines if a developer is adhering to Test-Drive Design practices;
  1. [Continuous GQM](http://csdl.ics.hawaii.edu/research/ContinuousGQM), developed by Christoph Lofi, which determines degree of satisfaction of the measures, questions, and goals in a GQM network;


## 9.0 Where to go next? ##

This concludes the Guided Tour of Hackystat, and we hope you've enjoyed it.  Depending upon your interests, you might also want to take a look at one or more of the following pages:

  * [Sensor Data Collection Tutorial](Tutorial_SensorDataCollection.md), which provides detailed instructions on downloading sensors and collecting sensor data.
  * [Project Management Tutorial](Tutorial_Projects.md), which provides instructions on how to define and use Projects in Hackystat.
  * [Software Project Telemetry Tutorial](Tutorial_SoftwareProjectTelemetry.md), which provides detailed instructions on downloading sensors and collecting sensor data.
  * [Software Project Portfolio Analysis Tutorial](Tutorial_ProjectPortfolio.md), which provides detailed instructions on using Project Portfolio analysis.
  * [Community](Community.md), which summarizes some of the ways Hackystat has been used in the past.
  * [Research Projects](ResearchProjects.md), which overviews some interesting future applications of the Hackystat Framework.
  * [Publications](Publications.md), which overviews some of the research publications involving Hackystat.