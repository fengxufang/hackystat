# 1.0 Overview #

Hackystat Version 8 provides infrastructure for a number of interesting research and development projects.  This page is intended to provide a very brief overview of a representative sampling of such projects.  Some of these projects may have active participants, and some may not.

If you are interested in participating in one of these projects, or have an idea for a different project involving Hackystat,  please do not hesitate to contact Philip Johnson (johnson@hawaii.edu) for more information.

Note that we have applied to participate in Google Summer of Code 2008, and this page serves as the "Ideas" page for that program.

# 2.0 Research and Development Projects #

Here are some project ideas, in no particular order.  Your thoughts and comments welcomed.

## 2.1 Sensor development ##

The simplest way to enhance Hackystat is to provide a sensor for a new tool.   Developing a sensor for a Java-based tool, such as IntelliJ Idea, is quite straightforward and may take only a few days to weeks.

Developing a sensor for a non-Java based tool, such as Visual Studio, may take a bit longer, since it requires you to build an interface to the SensorShell.  We have done this before in Version 7, so it's just a matter of porting to Version 8.

Examples of tools that we'd like to see sensors for include:  [Visual Studio](http://msdn2.microsoft.com/en-us/vstudio/default.aspx), [TeamCity](http://www.jetbrains.com/teamcity/),  [Structure 101](http://www.headwaysoftware.com/products/structure101/index.php), and [Bugzilla](http://www.bugzilla.org/), but there are many other interesting possibilities.

While sensor development might appear at first glance to be a somewhat
mundane "small matter of engineering", it can sometimes have far-reaching
impact on Hackystat.  For example, the Structure 101 tool implements a
measure of architectural complexity called "XS" with two submeasures: "fat"
and "tangle".  Developing a sensor for Structure 101 thus involves
designing an appropriate representation for that measure.  Such a
representation can lead to new research directions, such as investigating
the relationship between XS and other measures of complexity, such as
Chidamber-Kemerer object oriented metrics, cyclomatic complexity, or
UML-oriented design metrics.

## 2.2 Investigating Crap4J ##

The good folks at Artima have created a plugin for Eclipse called [Crap4J](http://www.crap4j.org/).  The idea behind this plugin is to combine a measure of complexity with a measure for testing coverage, such that code with high complexity and low coverage is viewed as "crappy".  I wrote a little bit about this in [Hackystat and Crap4J](http://philipmjohnson.blogspot.com/2007/10/hackystat-and-crap4j.html).

There are several limitations of Crap4J:
  * It works only within Eclipse.
  * It works only for Java.
  * It calculates complexity only on the basis of within-method path complexity, and does not take into account, for example, coupling or other architectural-level complexity.
  * There is no empirical evidence that this metric has any value, or under which contexts it might have value, even though it "feels intuitively meaningful."

Hackystat provides infrastructure for enhancing and evaluating this measure in several ways.

**Provide an IDE and language independent version of Crap4J**

To accomplish this, one must create a Hackystat analysis and user
interface that computes and displays the Crap metric by retrieving Coverage
and Complexity data from the SensorBase.

First, this decouples the Crap4J metric from Eclipse, so that any
combination of tools that can send Coverage and Complexity data to
Hackystat can produce the Crap metric.  For example, this would enable
someone using Ant (via Emma or Clover for Coverage, and JavaNCSS for
complexity) to obtain the Crap metric for their code.

This would also enable the Crap metric to be calculated for any other
language in which coverage and complexity sensors exist.  For example, a
Visual Studio sensor that could send complexity and coverage data to Hackystat would
allow a developer to calculate the Crap metric on C#.

**Provide a "Crap++" metric that includes coupling**

The authors of the Crap metric acknowledge that it does not take into account other forms of complexity, such as module coupling.   The goal of this project is to enhance the original Crap metric to take into account such orthogonal complexity measures.

To do this, one would provide a sensor in Hackystat for a tool that
computes module coupling metrics, such as JDepend or DependencyFinder.
Then, one would investigate ways to extend the computation of Crap to
factor in results from the coupling data.

Determining the effectiveness of such an enhancement is the next step, discussed below.

**Perform an empirical investigation of Crap**

While Crap seems intuitively meaningful, there are currently no empirical studies that provide evidence for its utility or effectiveness.  The goal of this project is to provide such evidence.  Here are several potential research questions.

  * Does the Crap4J have any predictive or explanatory power? For example, does the Crap metric correlate with other measures of software quality, such as build failures or defect rates?

  * Does enhancing the Crap4J metric with other complexity information, such as Dependency, make any difference to the metric?

  * Are some languages more crappy than others?  Do some environments, such Idea, lead to less crappy code than other environments, such as Eclipse?

## 2.3 Ambient Devices in Software Development ##

Hackystat provides excellent infrastucture for investigating the application of ambient devices such as the [Ambient Orb](http://www.ambientdevices.com/cat/orb/orborder.html)  or [Nabaztag](http://www.nabaztag.com/en/index.html) to software development.

The goal of this project is to create a Hackystat service implementing a
"programmable user interface" to one or more ambient devices in order to
communicate Hackystat process or product analysis results.  The user
interface could, for example, provide a kind of "visual programming" environment, with
process/product data icons on one side, ambient device icons on the other
side, mouse gestures drag an arrow from one side to the other to "hook up"
the process/product data icon to the ambient device that will respond to
it.

At the simplest level, such an interface might provide  "on/off" kinds of triggers:  For example,  if the most recent continuous integration build for a project passed, it glows green, if not, red.
Details of this approach are available at [Bubble, Bubble, Build's in Trouble](http://pragmaticautomation.com/cgi-bin/pragauto.cgi/Monitor/Devices/BubbleBubbleBuildsInTrouble.rdoc) and
[Automated Continuous Integration and the Ambient Orb](http://blogs.msdn.com/mswanson/articles/169058.aspx).

What makes Hackystat integration interesting is that Hackystat can collect and integrate information from a wide variety of tools and developer behaviors, resulting in the potential for much more sophisticated information display in the ambient device. Here are a couple of examples:

**Commit Conflict Early Warning System.**

For example, assume you are a software developer working on a project in a
distributed development environment.  The tasks are quite interdependent,
and so there is a high likelihood that other developers may occasionally be
making changes in the same module or file at approximately the same time as you are.
The typical result of such concurrent activity is commit conflicts, which slow down development
by requiring everyone to back out their changes and figure out how to
re-integrate successfully.

In this scenario, an ambient device might be helpful by providing a way to
inform developers that they have started to edit code in a file that has
also been edited by another developer (who has not yet checked in his or
her changes.)  In a sense, the ambient device provides an "early warning
system", enabling a developer to immediately contact the team to find out
who is editing the file and discuss how to coordinate their changes before
the commit occurs.  Hackystat provides value in this scenario by providing
an IDE-independent implementation.

**Project Portfolio Progress Indicator.**

As another example, consider a manager of a large number of software
development projects.  There may be several "indicators" that a project
requires attention, including unexpectedly low or high developer
activity; gradually decreasing test coverage; an unusual number of build
failures; or unusually high code churn.  The typical approach is to
provide a "dashboard" with a set of charts showing all of this data.  That
solution, however, takes up screen real estate and requires the manager to
actually notice the change.  An ambient device might provide a superior
alternative, in which a green light indicates all of the indicators are
normal, along with a variety of non-green indicators to indicate what has
deviated from appropriate conditions.  Only when the light is not green
does the manager need to consult the dashboard to determine what to do
next.

**Integration with other ambient modalities.**

Some research has already been done on [auditory ambient feedback](http://citeseer.ist.psu.edu/295834.html).  What kinds of benefits accrue from combining visual with auditory feedback?

**Assessing ambient devices for software development.**


Ambient devices are totally cool, but do they actually add value?  How does
the use of an Orb compare to providing the same information via email, or
Twitter tweet, or cell phone text message?  Under what circumstances would
one of these communication media be preferable to another?  Hackystat can
provide infrastructure for investigating this question by implementing all
of these interfaces.  The next step is to devise an experimental design for
the collection of useful evidence regarding the strengths and weaknesses of
these mechanisms.

There is an interesting Java-based server for Nabaztag: http://www.cs.uta.fi/hci/spi/jnabserver/

## 2.4 A Hackystat/Micro-blogging Mashup ##

Micro-blogging services such as Twitter or Jaiku provide an interesting communication mechanism that has interesting implications for software development.  I wrote about this for the first time in
[Project Proprioception](http://philipmjohnson.blogspot.com/2007/08/project-proprioception.html), later
in [Twitter, Hackystat, and solving the context switching problem](http://philipmjohnson.blogspot.com/2007/09/twitter-hackystat-and-solving-context.html)
and once again in
[Measurement as Mashup](http://philipmjohnson.blogspot.com/2007/11/measurement-as-mashup-ambient-devices.html).   Members of the Hackystat development team have been using Twitter since Fall, 2007 as one of their communication methods, although without any direct Hackystat integration.

The basic goal of this project is to create a Hackystat service that can detect  "interesting
events" in the Hackystat data stream and then communicate them as Twitter or Jaiku micro-blog entries.

To understand why this might be interesting, consider the canonical question that a Twitter user is asked to answer:  "What are you doing now?".   Hackystat integration allows automated generation of a relatively rich set of software development-related tweets on behalf of a developer. Here are just a few examples:

  * I have just started editing Foo.java.
  * I am stepping through the source code of Bar.cc.
  * I just got a unit test failure in Baz.py.
  * I am resolving a commit conflict in Qux.cl.

A developer might well follow up on the automated tweet with a regular tweet in which they provide supplemental information or request help from others.

There are several interesting research questions to address in this mashup.
First, it is easy to imagine that such automated tweets could be little
more than "spam", so the first issue is determining how to keep the
signal-to-noise ratio sufficiently high that the service is considered
useful.

A second research issue is to evaluate the impact of such a mechanism on
software development. Does it lead to concrete improvement in developer
communication and coordination, or is it simply a neat idea with no real
benefit?

Finally, one can compare various micro-blogging services. For example, the "channels" mechanism in Jaiku could provide a superior way to organize this information over the non-channeled approach in Twitter.

## 2.5 FFTs and Telemetry ##

One interesting question about telemetry streams is whether there is "signal" in the trend line variations.  Dan Port suggested that we try running Fast Fourier Transforms over telemetry data to look for patterns, which I wrote about in  [FFT of Telemetry Streams](http://philipmjohnson.blogspot.com/2007/10/fast-fourier-telemetry-transforms.html) .

This research project would involve first implementing an FFT-based analyses, then performing an empirical investigation to see if meaningful patterns in the telemetry streams can be identified.

## 2.6 The Semantic Web and Software Development ##

I wrote some initial thoughts on how the Semantic Web (a.k.a. Web 3.0) could apply to software development in
[Hackystat and the semantic web](http://philipmjohnson.blogspot.com/2007/07/empirical-software-engineering-and-web.html).

As I noted in this article: It seems to me that Hackystat sensors are, in
some sense, an attempt to take a software development artifact (the sensor
data "Resource" field, in Hackystat 8 terminology), and retrofit Web 3.0
semantics on top of it (the SensorDataType field being a simple
example). The RESTful Hackystat 8 services are then a way to "republish"
aspects of these artifacts in a Web 3.0 format (i.e. as Resources with a
unique URI and an XML representation) . What is currently lacking in
Hackystat 8 is the ability to obtain a resource in RDF representation
rather than our home-grown XML, but that is a very small step from where we
are now.

And here are some possible research directions:

  * Can Web 3.0 improve our ability to evaluate the quality/security/etc. of software development projects?
  * Can Web 3.0 improve our ability to create a credible representation of a programmer's skills?
  * Can Web 3.0 improve our ability to create autonomous agents that can provide more help in supporting the software development process?

## 2.7 Social Networks and Software Development ##

In [Social Networks for Software Engineers](http://philipmjohnson.blogspot.com/2007/12/social-networks-for-software-engineers.html), I provide examples of how Hackystat sensor data can be used to develop relatively detailed representations of the skills and capabilities of software developers, and how these representations could be used within a social network to support question answering, community building, and professional development.

For this research project, the first step is to develop an enhancement to an existing social
network infrastructure such as Facebook or Orkut that can provide developers with
the ability to publish such profile information and make queries involving
it.

The second step is to perform an evaluation that helps reveal the strengths and weaknesses of this approach to social networking for software developers.

Hackystat has a [Facebook Group](http://www.facebook.com/group.php?gid=7429464774).

## 2.8 Is Ruby on Rails really better than Java?  Is NetBeans better than Eclipse? And so forth. ##

While there is copious debate in the blogosphere over the strengths and weaknesses of different languages and environments, there is relatively little empirical evidence.

Hackystat provides infrastructure that can generate detailed data about the
use of languages (Ruby, Java, PHP, etc.) and environments (Rails, Grails,
Eclipse, NetBeans, etc.)

The first step in this project is to determine what to measure between the two languages/environments under comparison.
The second step is to design an experimental methodology that generates useful evidence regarding the differences between these environments.

A conventional approach to such a study is to specify a set of "outcome"
measures or independent variables---code size, complexity, functionality,
test coverage, etc. and then attempt to compare "similar" projects in which
the dependent variable is the language or environment.  For example, you
might design a study to see if a project written in Ruby on Rails contains
much less code than a similar project written in Java.

A different approach is to study the behavioral characteristics of
development and how they are influenced by the choice of language or
environment.  For example, what are the sequence of editor and/or shell
commands required to set up a project in Ruby on Rails, and how is this
different from what is required to set up a Java web application project?
What is required to implement a "small change", and how is this different
between Ruby on Rails and, say, Struts?

In either case, important experimental design issues exist.  For the first
approach, an issue is determining whether the two projects are actually
"similar", and whether the outcome measures are due to the dependent
variables or rather just due to differences in programmer skill.  For the
second approach, an issue is to differentiate effectively "random"
variation in behaviors, or application-specific behaviors, from behaviors
that are "essential" and due to the infrastructure itself.

These are difficult, but not insurmountable experimental design issues.


## 2.9 A Hackystat/Ohloh Mashup ##

[Ohloh](http://www.ohloh.net) is, according to their home page, "an open source network that connects people through the software they create and use."  In practice, Ohloh works in the following way:

  * Software developers create an account and point Ohloh at their version control repository.
  * Ohloh crawls the repository periodically and counts the code, looks for file types, licenses, and committers.
  * The Ohloh project home page presents  some general statistics and summaries for the project.
  * Developers provides review of projects and kudos points for other developers.

For example, here is the [Hackystat Version 8 Ohloh page](http://www.ohloh.net/projects/5584?p=Hackystat-8).

One of Ohloh's goals is to provide a sense for the credibility
of an open source project.  For example, for Hackystat 8, the summary is:

```
Mostly written in Java
Large, active development team
Well-commented source code
Short source control history 
Apache software license may conflict with GPL
```

An interesting synergy might result from enabling Ohloh to display
information collected by Hackystat sensors as a way for developers to
improve the credibility of their project to the outside world.  For
example, it would be quite nice to know the test case coverage of an open
source project, and even better, to know the history of its coverage over
time.  It would be quite problematic for Ohlo to actually build every
system, run its test cases, and generate coverage, given the multitude of
languages, technologies, and reporting mechanisms.

That is not a problem for Hackystat, since it provides sensors for
various build and testing tools in a variety of languages, which
means that if the development team is actually running test cases
and generating coverage data internally, Hackystat provides a way
to make that data externally available in a standard way.  A second
example is test-driven design (TDD), for "agile" organizations,
being able to provide credible evidence that they are employing a
practice like TDD can be a major selling point for their product.
There are lots of other possibilities as well.

An initial approach to this project would be to make the Ohlo dashboard
extensible, with the ability to incorporate not only the "standard"
kinds of analyses that can be generated from analyses of the
configuration management repository, but with the ability to also
incorporate "extended" analyses based upon process/product data
that it can extract from a Hackystat repository associated with the
project.  Examples of extended analyses that Hackystat could
provide include:

  * test invocations
  * test coverage
  * daily build results
  * static analyses from tools such as PMD, Checkstyle, FindBugs,  Lint, etc.
  * code review results
  * coupling and cohesion
  * performance analysis
  * developer effort

The more kinds of extended analyses that a Project makes available
to Ohlo, the higher the quality of the evaluation that Ohlo can
perform on the project, and the more confidence a prospective user
of an open source project might have in using it in their own work.

There are privacy issues that need to be worked out, to be sure.
Currently in Hackystat, data on a Project is visible
only to those developers working on that Project.  For this kind of
synergy to exist, it must be possible to allow developers to easily
specify "public" aspects of their Hackystat data that they will
allow a service such as Ohlo to obtain. Given that, it seems like
the interface on the Ohlo side is relatively simple and similar to
the current enlistment mechanism: just as one currently specifies
an SVN server, one would add a specification of a Hackystat
server.

I have already communicated these ideas to the Ohloh development team, and
they are enthusiastic about the possibilities. Let me know if you want to
pursue this and I will followup on my prior communications.

## 2.10 A Continuous Integration "Build Game" ##

In [The Continuous Integration Build Game](http://clintshank.javadevelopersjournal.com/ci_build_game.htm), Clint Shank outlines an approach to a "game" that enables developers to gain points for practicing good continuous integration practices.

I think we could use Hackystat to build upon his approach to provide a useful and educational way to learn good incremental development and configuration management practices.

Building on his initial idea, implement a Hackystat service that provides a running scoreboard for each member of a development team. Each "player" would get some number of positive points each time they check in code according to criteria like the following:

  * There were no commit conflicts.
  * The build compiled.
  * All test cases passed
  * No static analysis tool (Checkstyle, PMD, FindBugs, etc.) generated an error.
  * Coverage increased, or else was over some threshold and did not decrease.
  * The "volume" of the checkin (code churn) was both above some threshold value and below some other threshold value.
  * The "elapsed time" since the last checkin was both above some threshold value and below some other threshold value.

Conversely, points can be taken away from a player when the checked in code violates one or more of these conditions.

Note that assessing the appropriateness of "elapsed time" will require some sophistication.  A continuous integration rule of thumb is that "a checkin a day keeps the integration failure away".  However, the system has to take into account that if a developer isn't even working on the code, then they shouldn't be penalized for several days elapsing before they do a checkin.

Similarly, "volume" has to take into account the fact that refactoring can generate a large amount of code churn in a short period of time.  So, if a player is making a commit per day, and one of these commits has very high code churn, then they shouldn't be penalized, since it's unlikely that they could or should have prevented this via more frequent checkin.  On the other hand, if the DevEvent data indicates that they were working on the system every day for the past week and never checked in any code, and then checked in a bunch of changes, that presumably indicates that the developer should have broken up the task into smaller pieces and checked in intermediate results.

This seems like a very reasonable one semester project.  It involves the following steps:

  * Install and learn a continuous integration server.  [Hudson](https://hudson.dev.java.net/) is getting good reviews these days.

  * Configure a sample project's Ant files so that when Hudson runs the build, specially tagged data goes to the SensorBase server.  We need to be able to distinguish between the Build, CodeIssue, UnitTest, and Coverage data generated by Hudson from the data generated by the local build.  We also need to know which developer's checkin was responsible for generating this data.

  * Build a Hackystat service for playing the game with a web-based front end.  This would enable developers to sign up to have their data analyzed, query the SensorBase for the data and perform the analyses, provide a scoreboard, and display some kind of "explanation" mechanism so that developers can learn why their practices were resulting in their points going up or down.  Use your favorite web application environment to build it: Ruby on Rails, Wicket, Symfony, etc.

  * Evaluate the game in a software engineering class, such as ICS 413 in Fall, 2008.  The goal is to deploy the game in the classroom, and conduct a survey of the participants afterwards to find out: (a) was the game fun to play; (b) was the scoring system appropriate and fair?; (c) were there ways to manipulate the game to inappropriately improve the score?; (d) did the game help you to learn good development practices?; (e) how could the game be made better in the future?

## 2.11 Integrating Qualitative and Quantitative Information through Project Timelines ##

[Simile/Timeline](http://simile.mit.edu/timeline/) is a visualization tool for time-based events.  It appears to be complementary to software project telemetry, in that rather than showing trends in numeric values over time, resulting in a kind of "continuous" data stream, Simile/Timeline instead shows "discrete" events and descriptions of them.

The Simile/Timeline visualization system provides an interesting way to investigate the blending of qualitative and quantitative information in software development.  One approach is to provide a client-side tool that can send a new kind of sensor data which contains a user-written, timestamped "annotation".  These annotations serve as a kind of developer log that provide context to other information being collected on a more automated basis.   Annotations can provide explanations for abrupt changes in telemetry trends, such as significantly decreased (or increased) testing coverage, or build failures, or code complexity.

A goal for this project is to investigate how developers can best provide
additional insight into a project by annotating and augmenting the
quantitative information collected and displayed with conventional
Hackystat sensors.   Some of the research and development questions include:

  * The design and implementation of a client-side tool that can collect developer annotations.
  * Triggers for the client-side tool.  While the developer is free to invoke it independently, it would be more useful if the system could "recognize" interesting events in the data stream, bring them to the developer's attention, and provide them with an opportunity to annotate them.
  * The integration of annotations with other data display mechanisms.  For example, how to display annotations along with telemetry trend data?



## 2.12 Alternative Data Visualization with Processing ##

From their home page: "[Processing](http://processing.org/)  is an open source programming language and environment for people who want to program images, animation, and interactions."

For this project, use the Processing environment to design new forms of visual interfaces to the information collected by Hackystat.  The [Exhibition page](http://processing.org/exhibition/index.html) provides some interesting examples of what can be done by Processing.  For example, [AnyMails](http://carohorn.de/anymails/) visualizes your email stream as a set of microbes, and [Query Burst Visualizer](http://design.yahoo.com/project.php?pid=9) shows a spinning globe with streams of queries emanating from different locations.  One could imagine designing interface services for Hackystat that, for example, shows bugs as microbes, or DevEvents overlaid on a "map" of the system.

In general, this project consists of two parts:  (1) Use [Processing](http://processing.org/) to generate a wildly new way to interact with Hackystat data, and (2) Use [Science](http://en.wikipedia.org/wiki/Scientific_method) to figure out whether what you generated is actually helpful or just an amusing hack.

## 2.13 Blending physical and virtual workspaces ##

[Informative Workspaces](http://www.ddj.com/architect/204800604) tend to blend the physical and virtual:
"manually updated nonelectronic visual displays" co-exist with "electronic
devices hooked up to automated processes".    The result (as Kent Beck puts it), is "when
an interested observer walks into your team space, s/he should be able to get a general
idea of how your project is going in 15 seconds, and be able to get a deeper perspective
on real or potential problems by looking more closely."

### Using the CSDL Telemetry Wall ###

One straightforward way to explore this issue would be to investigate ways to exploit the existing nine monitor display in CSDL.  Aaron made an [interesting post](http://kagawaa.blogspot.com/2007/09/telemetry-wall-20.html) about the possibilities.

### Using Second Life ###

Another, more radical approach, would be to design and implement a team space in [Second Life](http://secondlife.com/)
as well as in the physical world? Some initial thoughts:

  * A globally distributed team in the physical world would be co-located in Second Life.  You could see "at a glance" whether a team mate was "in" or "out" of the office, and they could see whether you could see them there or not;

  * You could have the physical "co-located" cues in the Second Life space.  For example, someone's Second Life office door would be open if they are amenable to interruption, closed if not;

  * You could have much more "interesting" kinds of ambient devices, because you are no longer restricted to things like the laws of gravity or construction economics.  Instead of an ambient device glowing, you could have a lava lamp that levitates.  You could provide an indication of project status by the weather outside the office windows: sunny, warm, Hawaiian weather means everything is No Ka Oi, cloudy means problems on the horizon, snow up past the windows means time for the Death March.

Here's some interesting links:  [Software development in Second Life](http://dev.aol.com/article/2007/04/second-life), and
[Second Life Software Engineering Process Game](http://vital.cs.ohiou.edu/vitalwiki/index.php/Second_Life_Development).


## 2.14 Exploration To Publication: From Wicket to Swivel Business ##

One pattern of use in Hackystat is:

  1. Collect data.
  1. Explore various analyses on that data.
  1. Find an analysis of interest, and initiate a conversation about that analysis with colleagues.
  1. Repeat.

Over the years, we have worked hard to provide flexibility, generality, and efficiency with respect to data collection.  Analysis mechanisms such as Software Project Telemetry and Daily Project Data provide a large space for exploration of alternative analyses.  Our [ProjectBrowser](http://code.google.com/p/hackystat-ui-wicket/) provides a nice UI for configuration and exploration of the space of analyses.

Where we continue to struggle is in finding effective ways to support "conversations" once an interesting analysis result has been discovered.   One of the important findings of Hackystat is that effective use of the empirical data generally requires human interpretation.

The recent release of [Swivel Business](https://business.swivel.com/) provides an interesting platform to explore the "publication" phase. Swivel allows you to upload datasets, and then allow a wide variety of ways to visualize and annotate the results.

A very interesting research project would explore this pattern of use, by designing some sort of "bridge" between Hackystat analyses and Swivel Business.  Some issues to explore:

  * What kinds of analyses are useful to "publish"?  Some might be "static" (such as a dataset that shows some sort of anomaly in development during a single month). Others might be "dynamic" (such as an automatically updated dataset that shows trends in coverage for a set of projects.)

  * What kind of user interface provides an efficient and effective means for bridging the gap between "exploration" and "publication"?  It is pretty trivial to write a little script that gets some DPD data and uploads it in CSV format to Swivel.  It is less trivial to design an easy to use interface that does not require custom coding for every possible analysis alternative.

  * What kinds of conversations result from "publication"?  Once the data can be annotated, what do users write about?  Does the Swivel Business annotation mechanism provide useful features for our domain? What kinds of changes would make it more useful?

  * What are the privacy implications of "publication"?


## 2.15 Supporting the "Scatter Plot Quick Select" visualization ##

One of the interesting applications of Software Project Telemetry is the discovery of co-variances in trend lines.  For example, it can be interesting to know that in your organization, commit frequency is correlated with build failure, or that test coverage is correlated with field-reported defects, or that coupling is correlated with unit test invocation failure rate.

The problem is that there are typically dozens of possible telemetry streams, leading to hundreds or thousands of possible pair-wise combinations. Given this moderately large space of possibilities, the question is how to enable the user to eeasily understand and explore it.

Enter "Scatter Plot Quick Select", a visualization created by Ben Schneiderman.  A nice example of the use of this visualization approach is provided in the [Palantir Tech Blog for 9/16/2008](http://blog.palantirtech.com/2008/09/16/scatter-plot-quick-select/).  Their example visualization shows pairwise combinations for 10 financial metrics, and how one could drill down.

One way to provide this visualization within Hackystat would involve the following:

  * Create the UI within the ProjectBrowser as its own tabbed page.

  * Upon visiting the page, the user selects a time interval, granularity, one or more projects,  and a set of Telemetry Charts.

  * The system then retrieves all of the telemetry charts for all of the selected projects, and extracts the constitutent "streams" which form the basic unit of analysis.

  * Pairwise combinations of all telemetry streams are formed, and correlations are calculated.  If the correlation is close to +1.0, then the pairwise combination is associated with dark green.  If the correlation is close to -1.0, then the pair-wise combination is associated with dark red.  Intermediate values get intermediate colors.

  * A triangular matrix is generated with a single box, appropriately colored, for each pairwise combination, and presented to the user.

  * Clicking on a given box generates a chart with the two telemetry streams so the user can better understand the correlation.


