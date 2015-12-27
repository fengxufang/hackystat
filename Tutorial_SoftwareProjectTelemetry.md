## 1.0 Motivation ##

Hackystat sensors provide a way to collect very low-level data about software processes and products. For example, a DevEvent sensor data instance might indicate that a developer saved the file Foo.java at 11:59:59am on 2007-09-09.  Such low-level information, by itself, is very difficult to use in project management and decision making.   There needs to be a way to abstract this low-level data in such a way that it becomes meaningful.

This need for abstraction leads directly to the second problem:  which abstractions should be used?  After all, different organizations have different work processes, which means different abstractions will be meaningful to them.  One organization might be committed to the use of Test Driven Design practices, and so an abstraction that enables them to understand their compliance with TDD practices might be quite useful.  Another organization might not care about this abstraction.

Software Project Telemetry is an analysis mechanism for Hackystat that
provides one way of addressing both of these issues.  In order to make
low-level data useful for project management and decision making, it
supports the creation of trend lines that show how various characteristics
of software development are changing over time.  To support the work
practices of different organizations, it provides a domain specific
language that allows the creation of custom trend lines (called telemetry
"streams") and their visualization together in a specific telemetry
"chart".

The goal of this introductory tutorial is to show how telemetry can be
visualized in Hackystat using the ProjectBrowser  application and the built-in telemetry definitions, and how
these visualizations can reveal interesting characteristics of the software
development process.  To facilitate this presentation, will we present
"simulated" telemetry data.  Future tutorials will cover advanced aspects of software project telemetry, such as how to define custom analyses.

## 2.0 The Telemetry page in the Project Browser ##

The basic telemetry viewer in Hackystat is available as a page within the ProjectBrowser application.  You can find the public Hackystat ProjectBrowser at http://dasha.ics.hawaii.edu:9879/projectbrowser/.  You need to login to the public ProjectBrowser using your public Hackystat username and password.  Once you are logged in, you can click on the "Telemetry" tab to be taken to a page that looks like the following:

![http://hackystat.googlecode.com/svn/wiki/telemetry.startscreen.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.startscreen.gif)

This page has a variety of controls that enable you to specify what telemetry you are interested in:

**Chart**: The Chart menu enables you to pick what kind of chart to generate.  Each Telemetry Analysis service can be configured with a set of charts defined using the Software Project Telemetry definition language.  (This language allows organizations to define their own custom charts, but is beyond the scope of this introductory tutorial.) To see documentation on the available charts, you can click on the "?" icon beside the Chart pull-down menu, which will pop up a modal dialog box similar to the following:

![http://hackystat.googlecode.com/svn/wiki/telemetry.chart.help.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.chart.help.gif)

**Start/End Date**:  You specify the interval of time of interest using the Start Date and End Date fields.  Remember that aeach project is specified with a start and end date, and the dates you enter here must fall within the Start and End dates for the Projects you specify next.

**Project(s)**:  You can specify one or more projects in this multi-selection list window.

**Granularity**:  Telemetry can be specified at the granularity of Days, Weeks, or Months.  Some telemetry data turns out to be "noisy" when viewed at the Day granularity, and thus you get a more accurate perspective by viewing it at a higher granularity of weeks or months.  For example, DevTime data might be quite variable at the Day granularity, since developers might work a large number of hours on code one day, then not much the next.  But at the granularity of a Week, this variability should smooth out and trends in terms of rising or lowering DevTime should become more apparent.

**Chart-specific parameters**:  Following the Granularity pull-down menu are zero or more context-sensitive menus that change depending upon the selected Chart.  These menu items enable you to provide additional parameters to the chart of interest.  Each of these chart-specific parameters has an associated "?" icon that provides documentation on what the parameter means.

Once you have specified all values, you press "Display Telemetry" to generate a table containing the Telemetry Data.  Here is an example:

![http://hackystat.googlecode.com/svn/wiki/telemetry.productdevtrends.table.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.productdevtrends.table.gif)

As you can see,  we selected two projects (Default and simpletelemetry), and the Chart called "ProductDevTrends".  This built a table with 10 rows, five rows for the project "Default" and five rows for the project "simpletelemetry".  Each row represents a single telemetry "stream", or trend line.

Using this table, we can select one or more of the telemetry streams to be presented in a graphical chart using the checkboxes.  The top-most checkbox selects all of the streams.  In general, charts containing more than five or six telemetry streams tend to be hard to interpret, so we will pick a subset to display.  We could select streams within a single project, for example, DevTime Hours and Total LOC within the simpletelemetry project, which would enable us to visualize how DevTime and Size relate over time.  Alternatively, we could select streams across multiple projects, for example DevTime for Default and simpletelemetry, which would enable us to visualize how the developer's total DevTime relates to the total DevTime on the simpletelemetry project.  Let's visualize that one:

![http://hackystat.googlecode.com/svn/wiki/telemetry.productdevtrends.compare.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.productdevtrends.compare.gif)


Now that you understand the basics of the telemetry display mechanism, let's look at an example.

## 3.0 Understanding project characteristics through Telemetry: A simulated example ##

The Simple Telemetry scenario is designed to illustrate how Software
Project Telemetry can reveal various characteristics of an ongoing software
development project.

The Simple Telemetry scenario consists of four 10 day periods of simulated
project data during July 2007.

Each 10 day period of development can be thought of as a Scrum
"sprint". While real software projects typically have sprint durations
longer than 10 days, this approach helps provide the most inutitive
understanding of how software project telemetry can be applied in an agile
context.

This Simple Telemetry scenario involves two developers, Joe and Bob, who
are working together on a project called SimpleTelemetry. Their Hackystat
usernames are joe.simpletelemetry@hackystat.org and
bob.simpletelemetry@hackystat.org.

The scenario involves two kinds of charts: product-oriented charts that
focus on trends in the project as a whole, and member-oriented charts that
focus on the activities of individual members in the project. The
ProductDevTrends and ProductQATrends telemetry charts show trends in
coverage, size, churn, complexity, commits, builds, test invocations, code issues, and
DevTime for the project as a whole. The MemberTrends chart shows, for a
single developer, their trends in test invocations, DevTime, builds,
commits, and churn.

The telemetry for the four simulated sprints illustrates the following
development scenarios:

  * "Healthy Development" (Sprint 1, July 2-11, 2007). This first sprint shows what telemetry looks like in an idealized, healthy project development scenario. Coverage is consistently high; overall code size changes smoothly; Churn, Complexity, and Code Issues are low relative to code size; Commits, Builds, and Unit tests occur regularly; and DevTime is consistent, and at a "Goldilocks" level (not too high, not too low.)

  * "Late Start" (Sprint 2, July 12-21, 2007). This sprint shows how telemetry can reveal a late start and its potential impact. For the first half of the sprint, there is very little movement on the project. Then all of the indicators (DevTime, Churn, Complexity, Commits, Size) show a dramatic spike upward. However, the negative impact of the late start manifests itself in quality: coverage is low, and code issues are high.

  * "Design Trouble" (Sprint 3, July 22-31, 2007). This sprint shows how telemetry can provide an early warning signal. The telemetry shows decreasing coverage, increasing code issues, and high levels of churn. If a project has been generating telemetry similar to Sprint 1 and then shifts to these trends, it indicates that something fundamental about the project has changed, either in personnel, requirements, or management.

  * "Resource mismanagement" (Sprint 4, August 1-10, 2007). This sprint shows how telemetry can reveal unbalanced resource allocation. The telemetry shows that Joe is overworked and taking on too much of the project development burden, while Bob is essentially idle with respect to this project.


### 3.1 Sprint 1: Healthy Development ###

Let's begin with telemetry for a "healthy" project.  Here is the ProductQATrends telemetry chart for the time period associated with the first sprint:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1.gif)

As you can see, coverage is around  90%, unit tests are invoked consistently every day around 10 times, and code issues are quite low.

The ProductDevTrends for a "healthy" project shows similar consistency:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1b.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1b.gif)

Churn, Commits, Builds, and DevTime hours are fairly stable.  The only line trending upward is the total code size, which is growing almost linearly.  (Of course, a project can be "healthy" with a non-linear, or even decreasing code size, but it is important that the other metrics show a certain level of stability in a healthy project.)

Finally, we can take a look at the MemberTrends chart to get a perspective on the development behavior of individual developers.  Here's Joe's MemberTrends chart:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1c.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum1c.gif)

Joe's individual telemetry is quite similar to the project as a whole: all of the indicator are stable and within reasonable values.
Bob's individual telemetry chart is similar to Joe's, so we will not show it here.

### 3.2 Sprint 2: Late start ###

Now let's look at the telemetry for a scenario where a project gets a late start and then the developers try to play "catch up".  Let's start this time with the ProductDevTrends chart:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum2b.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum2b.gif)

Here you can see that there are very low levels of project activity for the first half of the sprint, and then frenetic activity for the last five days.

What impact does such a late start have on project quality?  Let's look at the ProductQATrends chart for a perspective:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum2.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum2.gif)


Although unit testing increased dramatically at the end of the sprint, it didn't have the same "yield" as in the healthy project: code issues went upward, and coverage never increased to a very high level.

The MemberTrends charts for both Joe and Bob also illustrate the late start, though we will not show them here.

### 3.3 Sprint 3: Design Trouble ###

Now take a look at the following chart. What do you see?

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3.gif)


This looks worrisome:  unit test invocations appear quite low, coverage is decreasing, and code issues are increasing!  Clearly, there is something wrong with this project.

Interestingly, the ProductDevTrends chart does not reveal anything unusual.

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3b.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum3b.gif)


### 3.4 Sprint 4: Resource mismanagement ###

For this final example, let's return to the MemberTrends charts.  First, let's take a look at Bob's MemberTrends chart:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.bob.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.bob.gif)

Bob does not appear to be working on the project at all; he must be allocated elsewhere.  What impact does that have on Joe?

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.joe.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.joe.gif)


Joe is overworked and trying to hard to accomplish two developer's worth of work.  Let's see what impact this has on project quality:

![http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.gif](http://hackystat.googlecode.com/svn/wiki/telemetry.scrum4.gif)

It's not good. Coverage is too low, code issues are too high and steadily rising, and this is despite a significant amount of daily unit testing.

## 4.0 Where to go next? ##

The goal of this introductory tutorial is to give you a sense for how telemetry can be used to gain insight into software development project characteristics.  Of course, real life projects won't generate telemetry as simple as these simulated examples.  The challenge is to determine what kinds of telemetry are useful for your own development situation, and when changes in telemetry values indicate important changes in your development process.

As you gain experience with the built-in charts, you will likely start to become interested in defining your own custom telemetry.  Contact the Hackystat developers group for support in that process. We plan to publish a tutorial on writing custom telemetry in the near future.

Finally, this example focusses on the use of telemetry to monitor and manage a single development project.  As noted at the beginning of this tutorial, you can select multiple projects in order to perform comparative analyses of different projects.  For a related approach to multiple project management, we have developed a [project "portfolio" analysis mechanism](Tutorial_ProjectPortfolio.md).

Finally, both telemetry and project portfolios enable you to compare projects that are co-occurring.  However, in some cases you might want to compare the characteristics of projects that occurred at different points in time.  We have a project "trajectory" analysis mechanism under way that can provide a means to accomplish this.  Details will be forthcoming.




























