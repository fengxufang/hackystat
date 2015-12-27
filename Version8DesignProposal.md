# 1.0 Introduction #

This document presents some of the high level goals for the next major
release of Hackystat. (This is not the first time I've done this: for
historical purposes, you might find it amusing to take a quick look at the
[Version 7](http://hackydev.ics.hawaii.edu/hackyDevSite/doc/VersionSeven.html)
and
[Version 6](http://hackydev.ics.hawaii.edu/hackyDevSite/doc/VersionSix.html)
planning documents.

# 2.0 Version 7 is great #

Before launching into a vision for the next major release of Hackystat, I'd
like to begin by reviewing some of the things that the current version of
Hackystat appears to get right.  For example, no system other than
Hackystat can do the following:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.fig1.jpg](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.fig1.jpg)

This single chart illustrates many of the unique features of Hackystat. At
one level, it shows how software project telemetry and software development
stream analysis can work together to reveal how coverage co-varies with the
degree of test driven design practice being done by this developer during
this particular time. More importantly, it does this in a maximally
leveraged way. In other words, (a) the telemetry mechanism was designed
without any concept of being applied to TDD; (b) SDSA was designed without
any a priori idea of looking at coverage trends; and (c) the Hackystat
command and chart interface was designed without any a priori idea of
looking at any of this.

Thus, despite the conceptual independence of these components, the
framework enables an integration that reveals a tangible, provocative
empirical trend in this programmer's software development.

Finally, and perhaps most importantly, (d) this particular chart was not
the product of some premeditated research hypothesis. It instead arose
completely opportunistically: Hongbing and I were chatting one day, and he
showed me a chart illustrating that his percentage of TDD episodes had
trailed off over a couple of months on one of his programming tasks.  After
talking about it for a few minutes, one of us wondered what was going on
with coverage during that same period of time. Twenty minutes later (most
of which was spent consulting the documentation on reduction functions and
the Chart telemetry definition syntax), we had the answer: coverage dropped
off in just the same fashion as the TDD episode percentage. This
illustrates a Big Idea of Hackystat: when data collection is cheap enough,
and when the analysis mechanism is robust enough, you can think up
interesting measurement questions and get the answers in minutes by
consulting your database of metric data and using the appropriate analysis.

My point is (and I do have one): Hackystat Version 7 has tremendous
functional capabilities. Unlike a few years ago, I no longer worry about
whether we have enough sensors, sensor data, and analyses to discover
interesting new things about software engineering: we clearly do, and we
are clearly discovering cool new things all the time. Furthermore, there
appears to be a "network" effect: new facilities (such as Zorro) tend to
synergize with previously built facilities (such as Telemetry). While I'm
psyched to increase the set of sensors, sensor data types, and analyses
even further, I feel we've gone past some sort of critical mass with
respect to the functionality and capability of the system.

# 3.0 Version 7 ain't so great #

So, if Version 7 is so wonderful, then why do we need Version 8?

We don't need Version 8 to improve the functional capabilities of
Hackystat. We need Version 8 to make the existing functional capabilities
of Hackystat **available** to your average developer.  The problem with
Hackystat is that all of this amazing functionality is buried within an
interface that makes it likely that you will devote a lot of time to
dealing with a lot of mistakes before getting everything configured
correctly to get the output you are looking for. Even worse, while making
those mistakes, the system might not give you much feedback on what you did
wrong and how to correct it.

With Version 7, there is always pain before gain. For example, before you
can get any interesting analysis out of the system, you have to define a
Project. To define a Project, you need Workspaces. To define a Workspace,
you need to define a Workspace Root. To define a Workspace Root, you need
to have already sent data from your Project. Huh? But wait, you haven't
even defined the Project yet! And what the heck is a "Workspace Root",
anyway?

Assuming that if you finally get the sensor working and your account
configured, then you are confronted with 78 different analyses split among
five different pages (admin, analyses, preferences, alerts, extras). Which
one to try? How do you set the parameters on them to get something useful?
If you get the "null" analysis back, what did you do wrong? Was it your
arguments to the analysis command? Was it the Workspace Root setting? Was
it the Project definition? Was it your sensor setup?  Was it the phase of
the moon?

# 4.0 Version 8: It's the Interface, Stupid #

The most important goal for Version 8 is to make the current functional
potential of Hackystat accessible to users. I propose that we achieve this
through a new kind of interface that changes the user experience through
the following three interface "design principles": (1) Customization, not
configuration; (2) Results are visible, the parameters are hidden; and (3)
Active, not passive.

## 4.1 Customization, not configuration ##

In Version 7, there is a large amount of unintuitive configuration required
on the server side in order to get analyses to display any information at
all.  In Version 8, all constructs should come with legal "default" values.
For example, everyone should have a "default" Project called "(default)",
which is defined to cover all of the data you have ever sent to the server.
The "default" Workspace value might be "", which matches all file
paths. With this in place, Hackystat will "work" straight out of the box
without having to "configure" anything on the server side.  The process of
obtaining more specific kinds of analyses on subsets of your data thus
becomes a process of "customizing" the existing default definitions, rather
than having to construct a legal definition from scratch, as is the case
currently.

## 4.2 Results are visible, the parameters are hidden. ##

In Version 7, the user interface initially presents a very long list of
analysis names with their parameters, such as the following:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.fig2.jpg](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.fig2.jpg)

The results of any of these analysis are displayed after entering a legal
combination of parameter values and clicking "Analyze".  This user
interface principle might be called "Parameters visible, results
hidden".

Now consider, in contrast, the
[Google Home Page](http://www.google.com/ig)
and its associated Widgets.  These widgets literally and
figuratively reverse the Hackystat relationship between parameters and
results.  For example, the Weather widget initially displays a three day
forecast by default:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.google.weather.png](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.google.weather.png)

The "Results" are thus what is initially displayed in the user interface.
If you don't like the default analysis results or presentation, you can use
the down arrow to bring up a configuration page:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.google.weather.2.png](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.google.weather.2.png)

The Version 8 user interface should emulate this user interface design
principle, in which analyses whenever possible present themselves to the
user initially in the form of "Results", not Parameters.

## 4.3 Active, not passive ##

Analyses in Version 7 have an extremely "passive" feel to them. Users are
required to continuously "poke" the interface in order to get it to do
anything. First, you poke the analysis with parameter settings, then you
poke it with the "Analyze" button to get some analysis results, then you
poke it again later to get an updated set of values.  The overall vibe is
as if Hackystat doesn't really want to do anything and you have to be
continuously prodding it into action.

In Version 8, analyses should have an "active" feel to them: they should
know how and when to update themselves automatically, without user
intervention. (This might be implemented through a default auto-update
interval value that the user can customize).  Going back to the Weather
example, the widget displays a three day forecast starting with the current
day, and if you leave it displayed, it will actively update itself each day
to display the next three days, or intermittently during the current day if
the forecast changes.

All three of these user interface changes are related to each other:
without default values for all of the constructs such as Projects,
Workspaces, and so forth, and without the ability to actively update
themselves, it is not possible to create Hackystat analyses that display
interesting information without user parameterization.

## 4.4 Non-monitor interfaces ##

For certain situations, achieving the usability and accessibility we would
like in Version 8 might require breaking out of the standard "web browser"
interface.  For example, there are a growing number of non-monitor-based
interface display mechanisms, such as "ambient" interfaces like the
[Orb](http://www.ambientdevices.com/cat/orb/orborder.html)
or
[Nabaztag](http://www.nabaztag.com/en/index.html).

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.orb.jpg](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.orb.jpg)![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.nabaztag.jpg](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.nabaztag.jpg)

One obvious application of these devices is an ambient
indication of project "vital signs". How to configure a
software engineering analysis to send data to an ambient device, and how it
will be displayed once there, is a interesting topic for research. As
a simple example, Quality Labs has produced an
[Ant task interface](http://qualitylabs.org/projects/ambientorb/)
to the Ambient Orb.

# 5.0 Information architecture implications #

These user interface changes are not superficial: they point toward the
need for a deeper look at the information architecture of Hackystat and how
to facilitate access to data at a variety of levels of
abstraction. To see this, consider the following diagram:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.1.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.1.gif)

This diagram provides one perspective on the way that information is
represented and processed in Hackystat.

At the lowest level, there is Sensor Data and Sensor Meta-Data.
Sensor data is collected by sensors and sent to the server
automatically. Three examples of Sensor Data are FileMetric data,
DevEvent data, and Commit data.

Sensor Meta-Data is data that helps organize and provide useful
contextual information about the sensor data. Examples of Sensor
Meta-Data include data about the user (their email address, user key, and
any "preferences"); Project data, which provides a means to
aggregate subsets of sensor data; and Workspace meta-data, which provides a
way to understand the sensor data in a platform-independent manner.

Sensor data and meta-data are processed to generate a variety of
intermediate "abstractions". In more traditional software metrics
terminology, these can often (but not always) be interpreted as
"measures" or "metrics".

For example, DailyProjectData refers a class of abstractions that compute
some property of a Project for a specific day. For example, the FileMetric
DailyProjectData abstraction represents the size of the Project source code
on a specific day. This requires processing the Project sensor meta-data
(in order to determine what FileMetric sensor data is of interest) and the
FileMetric sensor data (in order to accumulate the required size data).  At
the highest level, sensor data, sensor meta-data, and intermediate
abstractions are combined into Analyses which are presented to the
developer for interpretation. For example, a Telemetry stream might show a
sequence of DailyProjectData FileMetric abstractions for thirty days,
illustrating the trend in software size. While Analyses are often based
upon Abstractions, they may also directly reference sensor data or sensor
meta-data.  Currently, these information architecture components are
encapsulated, or "bundled", within the Hackystat web application, as
illustrated next:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.2.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.2.gif)


There are a number of interesting limitations of this design. First, as
you can see, there is one interface designed for tool-oriented manipulation
(SOAP), and another interface designed for human-oriented manipulation (Web
Pages). The tool-oriented interface focuses on provision of sensor
data to the web application. The human-oriented interface focuses on reading
and writing of sensor meta-data, and read-only display of
Analyses. Left almost completely out of the picture is access to
the intermediate abstractions or even the raw sensor data.

What we have found through feedback from users over the years is a wish for
Hackystat to be more "open", in the sense that it would be easier to access
and extract all of the information components. Put another way, users would
like Hackystat to be able to function as a component in a "chain" of tools
that lead to the user:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.3.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.3.gif)

The current information architecture is not particularly friendly to this
goal. While we have put a fair amount of energy into making it easy to get
data into Hackystat (such as the XmlDataSensor), we have not put an
equivalent amount of energy into making it easy to get data out of
Hackystat. Put another way, Hackystat Version 7 does not "Play Well With
Others."

Fortunately, this is not an insurmountable problem; one could imagine
incorporating a variety of Web Service interfaces into existing Hackystat
web application, which would enable other tools to query the server for
information at all levels of abstraction:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.4.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.info.4.gif)

The dashed arrows show the new forms of information flow. Notice that the
"Tool" is now on an even footing with the "User" with respect to
information flow: both Tools and Humans can produce and consume the various
types of data in the system.

Let's go one more step and assume that all communication between the
principle information components occurs as web service calls. When we get
to this point, there is no longer the requirement for a single web
application; we could instead create the Hackystat system as a set of
cooperating software services, which might be distributed across multiple
machines and environments:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.services.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.services.gif)

Notice that there is no longer a "box" around Hackystat, and
that in fact the Hackystat "architecture" has become basically a
set of web service protocols along with reference implementations.

# 6.0 From information architecture to system architecture #

The previous discussion is intended to motivate our approach to Version 8,
but the diagram above doesn't work well as a system architecture. First,
"sensor data" and "sensor meta-data" are shown as two standalone entities,
but in reality neither of them make much sense without the other. Second, a
single box aggregates together all of the "Abstractions", and a single box
aggregates together all of the "Analyses/User Interfaces".  This isn't
right either: the "Episode" abstraction is completely independent from the
"DailyProjectData" abstraction, and this conceptual independence should be
manifested physically.

The following diagram illustrates the proposed system architecture for
Hackystat Version 8:

![http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.arch.gif](http://hackystat.googlecode.com/svn/wiki/Version8DesignProposal.arch.gif)

In Version 8, Hackystat is composed of a set of cooperating services (which
is popularly known as a "service oriented architecture, or SOA). There are
four basic flavors of services: Sensor services, SensorBase services,
Analysis services, and User Interface (UI) services. The "boxology" in this
diagram can be interpreted as follows:

  * Boxes are processes. Each box represents a physically distinct process. There is no architectural requirement for any two processes to be located on the same machine.

  * Arrows are APIs for information flow. Each arrow represents both an information flow and an API.  The arrow's direction indicates the flow of information from producer to consumer. Each arrow is numbered, where each number indicates a distinct API. For example, the arrows numbered (1) refer to the API that the chosen RDBMS must support in order to communicate successfully with a SensorBase Service. The arrows numbered (4) indicate the API that any SensorBase Service provides so that other services can query it for information. Unlike the SensorBase service, in which there is a standard API that all implementations must satisfy, each Analysis service can design and implement its own specialized API that is best suited to communicating its results to other services.

  * There can be arbitrary numbers of service instances. This diagram does not imply that Hackystat Version 8 consists of exactly 4 Sensor services, 1 SensorBase Service, 3 Analysis services, and 4 User Interface services.  Rather, it illustrates an example Version 8 configuration. In general, there is no limit to the number of Sensor, Analysis, UI, or SensorBase services that could be present in a Hackystat installation.


This architecture creates several new "degrees of freedom" for future
Hackystat development.

First, with respect to the top-level user interface, the architecture is no
longer strongly biased toward a Java webapp interface. Instead, by
providing public APIs to the SensorBase and Analysis services, user
interface services can be built using a variety of interface
technologies. We hope this will enable new experimentation with alternative
user interfaces that facilitate more effective and efficient use of
Hackystat data and analyses.  There is no longer a need to distinguish
"analyses" vs. "alerts", or "web applications" vs. "ambient devices".
These are all simply UI services.

Second, intermediate Analyses are no longer encapsulated within a Java
webapp implementation; they can be implemented in other languages or using
Java and inside or outside of a webapp framework. We hope this will
facilitate external implementation of analyses based upon Hackystat sensor
data by significantly lowering the "barrier to entry".

Third, the conversion to an underlying RDBMS platform (as opposed to the
existing XML flat file approach) creates an additional level of access to
the data in the system. By default, Hackystat will use the JavaDB/Derby
RDBMS system provided in Java 6, but will provide the ability to use an
alternative RDBMS if an installation site prefers a different choice.

Fourth, this architecture enables an Analysis service to access more than
one SensorBase Service, which opens up the possibility of multiple
SensorBase Services running as part of a single Hackystat installation, as
well as "community level" abstractions which gather and publish data from
across a set of Hackystat installations. (Indeed, a natural candidate for
an Analysis service is one that "sanitizes" the data for external
distribution.)

Fifth, in this architecture, there is no need to explicitly refer to
external "Tools" as was done in the information architecture diagrams based
upon Hackystat 7.  This architecture does not need to distinguish between
"internal" Hackystat tools (such as Analysis services) and "external"
Hackystat tools (such as editors or other client-side tools).  Indeed, a
single tool (such as Eclipse extended with an appropriate set of plugins)
could potentially serve as a Sensor service, an Analysis service, and a UI
service all at the same time!Sixth, each service can decide upon its
licensing scheme independently of any other service. There is no need for
all services to be based upon a single open source license, or even for all
services to use open source licensing! (We, of course, will continue to
publish all of our software using an open source license.)


# 7.0 From system architecture to design implications #

## 7.1 REST architecture ##

One significant impact of the Version 8 architecture is much greater degree
of inter-process communication.  A simplifying feature of all previous
Hackystat implementations was its simple client-server architecture, where
interprocess communication was essentially limited to a single one-way
transmission of sensor data from client tools to the Hackystat server web
application.  From there, all "communication" from low-level sensor data processing
up to the presentation of data occurred in Java via method invocations.

Version 8, in contrast, decomposes the Version 7 web application into a
potentially large number of coordinated processes/services which must
communicate with each other in a variety of ways.  Furthermore, many of
these services will both consume information from lower levels of the
system as well as produce information for higher levels of the system.

The presence of a multiplicity of communicating processes, and the need to
both produce and consume information, poses two design challenges: what
kind of inter-process communication protocol should be adopted, and what
kind of infrastructure can make this communication simple as well as
efficient and scalable?

A promising candidate is the
[REST architectural style](http://en.wikipedia.org/wiki/Representational_State_Transfer)
in combination with the
[Restlet framework for Java](http://www.restlet.org/).

The REST architectural style has gained great popularity in recent years as
an alternative to a remote procedure call approach, which is typically
implemented using SOAP.  In contrast, REST relies upon the HTTP protocol,
combining resources (URLS) with the standard HTTP primitives (GET, PUT,
POST, DELETE). When implemented correctly and appropriately, a system using
the REST architecture has many appealing properties.  First, interoperation
with other applications is relatively easy compared to RPC, since
communication is accomplished via standard HTTP calls, and resources are
specified via URLs.  Second, improving scalability, such as through
caching, can often be accomplished transparently through the introduction
of net-based caching of URL contents without any changes to the application
itself.

The Restlet framework for Java is a lightweight, scalable library that
makes it easy to implement REST-based communication.  Importantly, the
Restlet framework provides both client and server communication mechanisms,
so that Java-based processes using it can both produce and consume
information.

The REST architecture provides an organizing principle for the arrows in
the diagram above, and the Restlet framework provides supporting
infrastructure for Java-based implementations of non-Presentation layer
services. (We assume that if you are going to write a webapp, for example,
you're better off using Ruby on Rails, or Glassfish, etc. than the Restlet
framework.)  They do not provide details on the numbers attached to the
arrows; in other words, what are the specifics of the APIs used to
communicate between the various services?  Let's now start to drill down
into the details.

## 7.2 Sensor Data Types: From Build time to Run time binding ##

In Version 8, we propose to retain the notion of typed sensor data as in
Version 7. It is very important that there be standardized definitions
for the structure of sensor data. Without common definitions for sensor
data, there is the real danger of descending into an information chaos in
which each sensor sends data structured in an idiosyncratic manner that
makes comparison and interpretation difficult or impossible.

On the other hand, it is useful to allow some variability in sensor data to
accommodate the differing behaviors of tools within the same application
category. For example, the Eclipse editor provides more sophisticated
support for refactoring than the JEdit editor, and so it would be useful
for the DevEvent sensor data type to allow sensor data from Eclipse to
include such information about developer refactoring behaviors that could
not be captured in the JEdit editor.

In Hackystat, this tension between standardization and tool variation is
resolved through the use of "required" vs. "optional" fields in sensor data
type definitions.

Required fields for a sensor data type must be provided by all tools
sending data of that type, and these field values must obey the same
semantics. Required fields thus define the minimum amount of information
that all higher level analyses can rely upon when analyzing data of that
type.

However, sensor data of a given type can also include an unbounded amount
of "optional" fields. These fields might not be present in all sensor data
of that type, but support the ability to implement analyses specialized for
tools providing this information.

In Version 7, Sensor Data Types are a kind of meta-data, just like Projects
and Workspace Roots. However, while Projects and Workspace Root meta-data
are defined dynamically at run-time, Sensor Data Type meta-data is defined
at build time. Build-time definition of the set of Sensor Data Types works
well for an architecture where a single Java web application encapsulates
the sensor data, abstraction, and presentation services. It is problematic
in the Version 8 architecture, where these services are separate processes,
potentially implemented in different languages.

We propose that sensor data type information in Version 8 be defined and
managed dynamically, just as Projects, Workspace Roots, and other forms of
sensor meta-data are currently managed in Version 7.

## 7.3 Sensor Data: From Workspaces to URIs ##
In Version 7, all sensor data, regardless of its type, contains a set of
common fields. In Version 8, the set of common fields will be:
  * timestamp
  * runtime
  * tool
  * type
  * URI
  * userkey

The timestamp required field is a UTC long that indicates the time at
which the sensor data was generated. This value should be unique for each
sensor data entry.

The runtime field, in contrast, is a field that is
used to "bundle" related groups of sensor data together. For
example, FileMetric and Coverage data occurs in "batches", each
corresponding to a single run of the tool. Given the possibility that
tools generating FileMetric and Coverage data could be invoked multiple
times per day, it is important to be able to distinguish sets of these
sensor data instances generated at the same time. Although the runtime
field is not always needed, its use is widespread enough that we want to
elevate it to common status in Version 8.

The tool field is a string denoting the name of the tool that generated
the sensor data.

The type field is the name of the sensor data type
associated with this data.

The userkey identifies the user associated
with this data.

These are all well known fields from Version 7.
What changes in Version 8 is the representation for the
"location" of the sensor data. In Version 7, sensor data was
required to contain a "path" or "file" attribute, from
which a "Workspace" could be constructed. To construct a
Workspace, which was a platform-independent path string, each user was
required to define one or more "Workspace Roots". The sole
reason for all of this is to enable sensor data to be associated with one or
more Projects.

In Version 8, we will make a combination of design changes that will both
generalize and (hopefully) simplify the way in which sensor data is
associated with Projects. First, instead of a path string, sensor data
will provide "location" data in the form of a URI. Thus,
sensor data must be associated with a "resource", which could be a
path using the file:// URL syntax. However, it could be other
resources as well.

The change to URIs renders Workspaces, and their associated Workspace
Roots, obsolete. Workspaces and Workspace Roots will not exist in
Version 8.

However, this raises the question of how to identify sensor
data as a member of a Project?

## 7.4 Projects ##

In Version 7, Projects include the following characteristics:

  * A start day, and an end day (or null if no end day)
  * A set of users
  * A set of workspaces

Sensor data that satisfied all of these constraints were said to be
associated with that Project.
In Version 8, Projects can be defined in a manner similar to the Version
7 style:

  * A start day, and an end day (or null if no end day)
  * A set of users
  * A wildcard URI expression

The idea of the "wildcard" expression is to avoid the need to
specify workspace roots. For example, if all members of the project
agree to keep their Hackystat code somewhere inside a top-level directory
called "hackystat", then the following wildcard URI expression
might suffice:

  * [file://**/hackystat/**](file://**/hackystat/**)

This is clearly much more simple than the Workspace Root mechanism.
It also supports an implicitly defined personal "default" Project
for all sensor data, in which the wildcard URI expression is "".

Finally, it would be interesting to experiment with an additional field
that can contain an arbitrary expression written in a scripting language
like Groovy or Javascript. This expression would essentially define an
additional filter over the set of sensor data selected by the time interval,
users, and URI expression listed in the previous fields. It would enable, as
a simple example, a user to specify that only sensor data from a given
editor would be associated with this project, or only sensor data received
after 5pm.

## 7.5 Authentication ##

In Version 7, authentication consisted of "registering" with a
Hackystat server, which resulted in a 12 character userkey being sent to
your email address. This userkey was sent with your sensor data, and
could be used to construct URLs to access your data on the server. This is a
minimalist approach to authentication which is easy to implement and would
be quite compatible with REST, since the userkey could continue to be
encoded into the URL.

I'm not yet sure what to do about authentication in Version 8. One
approach would be to continue to use the exact same approach as in Version
7.

A second approach would be to use the Authentication HTTP header, as
discussed in
[REST based authentication](http://www.berenddeboer.net/rest/authentication.html)

A third approach would be to sign the request,
such as is done with
[Amazon Web Services](http://docs.amazonwebservices.com/AmazonS3/2006-03-01/RESTAuthentication.html),
or authentication tokens, as is done in
[EBay](http://developer.ebay.com/developercenter/rest/)
Token-based authentication is also discussed in the documentation for the
[Bla-Bla List](http://rifers.org/wiki/display/BLA/REST+API).

A fourth approach is to outsource authentication to a service like
[OpenID](http://openid.net/).
[OpenId4Java](http://code.google.com/p/openid4java/) is a Java
library for OpenID authentication.

A fifth approach is to combine some of the other approaches. For example,
we could begin by continuing to use our current scheme, and then layer
authentication on top.

## 7.6 Caching ##

One of the most interesting implications of a REST architectural approach
is that we can eliminate much, if not all, cache-related code from
Hackystat. Since all Hackystat data is a resource with a
corresponding URL, we can rely on internet caching mechanisms instead
of hand-rolling them ourselves.

Caching has been a major source of code size, complexity, and defects in
Version 7, and the ability to substantially eliminate this issue while still
providing scalability has the potential to be a huge win.

# 8.0 How to get there from here #

## 8.1 Become familiar with current user interface issues with Hackystat ##

It's important to gain a good understanding of all of the various pain
points in the current user interface. Conversations with current
users, as well as the
[2006 Classroom Evaluation](http://csdl.ics.hawaii.edu/techreports/07-02/07-02.html)
will be useful in this process. Nothing, of course,
replaces hands on experience with the current system.

## 8.2 Plan for evaluation of Version 8.0 in the Software Engineering classes of Fall, 2007 ##

An ambitious but reasonable goal is to have an initial implementation of
Version 8 (the 8.0 release) available for evaluation by August, 2007.

## 8.3 A proposed set of services to implement for 8.0 ##

It is infeasible, and also not necessary, to port all of the existing
Hackystat infrastructure for an August delivery of 8.0. Here are a
candidate set of 13 services:

**A SensorBase Service.** This service is the heart of
Version 8, and a minimal version must be the first piece of Version 8.0 to
be implemented. It will also require perhaps the most amount of new
code of all of the services scheduled for 8.0.

**Sensor Services: Eclipse, Ant, XmlData.** Migrating these
sensors to Version 8 will require minor modifications to the SensorShell
code so that it sends sensor data RESTfully. More extensive changes
will be necessary if Version 8 employs a different authentication mechanism
than the userkey-based technique used in Version 7. Once the
SensorBase service is available, these services should be made available and
bundled into an 8.0 compliant version of HackyInstaller.

**Analysis Services: DailyProjectData, Telemetry, SDSA/Zorro.**
These are the most important and useful abstractions currently supported in
Hackystat, and I would like to see them all available in Version 8.0 if
possible.

**User Interface Services.** An minimal initial set of UI services might include:

  * A **Registration service**, that enables a user to request a Hackystat account from a particular SensorBase service which will be received via email;

  * A **Sensor Data Viewer**, which enables a user to see in "near real time" when sensor data from that user is received by a SensorBase service. An interesting approach to this would be to use existing "chat" infrastructure such as Jabber.

  * A **Sensor Meta Data Editor**, which enables the user to create/read/update/delete meta-data such as Project definitions;

  * A **Project Status Viewer**, which provides the user with a "near real time" view of current project statistics (Dev Time, LOC, Churn, Commits, etc.);

  * A **Telemetry viewer**, which provides a "near real time" view of project trends using the Software Project Telemetry.

  * A **TDD Viewer**, which provides a "near real time" view of TDD episodes and classifications.

My current view is that GWT is the infrastructure most suited to
development of this initial set of 8.0 services, but that decision does not
have to be made quite yet.

## 8.5 Post Version 8.0 roadmap ##

The Version 8.0 schedule and deliverables is driven by the desire to
produce a system that can be evaluated by students in software engineering
in Fall, 2007. This will also help uncover bugs before the system is in wide
external use. Once Version 8.0 is available, development energies can
focus on other needs. Please let us know what you would like to see in the
8.1 release.

## 8.6 Infrastructure changes ##

Version 8 will be developed as a collection of [Google Project Hosting](http://code.google.com/hosting/) projects. This has both pros and cons:

Pros:

  * The 8.0 release will consist of 14 independent, but interrelated development projects. Creating infrastructure for each of these development projects internally would require significant resources.

  * Google Project Hosting provides support for Wiki pages and other useful infrastructure not currently available internally.

  * Using Google Project Hosting makes Hackystat appear more independent of UH and CSDL; it becomes more "open".

  * The physical resources (computers, backups, file space, etc.) required for the Hackystat Project going forward are outsourced to Google. This lowers the fixed costs of the Hackystat project to CSDL.

Unknown end tag for &lt;/li&gt;



  * Version 7 and Version 8 infrastructures are physically separated, making it easy to distinguish and migrate between them.

Cons:


  * Our current issue tracking mechanism (Jira) is far superior to the home grown issue tracker in Google Project Hosting.

  * Google Project Hosting does not provide a daily build mechanism. We will need to continue to provide this in-house.

  * Organizing Hackystat Version 8 as a collection of independent Google Projects introduces new coordination risks.