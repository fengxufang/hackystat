# 1.0 Introduction #

## 1.1 Portfolio management for software projects ##

In large companies, it is possible to have dozens or even hundreds of
software projects underway at any given point in time.  This kind of scale
produces new challenges, as well as new opportunities for these companies.
Both challenges and opportunities result from the need of the company to
successfully understand and exploit their ownership of a "portfolio" of
software projects.

While the principles and practices of software project portfolio management (SPPM)
are not well understood, one can gain some initial insight into its
potential through analogy with financial portfolio management, for which
there are much better established guiding principles.  Just like a
financial portfolio, a software project portfolio should be managed in such
a way as to balance risk against reward, support performance assessment in
multiple ways, and gain insight into each "investment".

In the financial world, investors have a variety of ways to gain insight
into their portfolio.  First, they have the annual reports of their
companies, in which representatives of a company provide details about the
current status and future directions of their organization.  Second,
investors can use objective, standardized metrics such as share price
appreciation (or depreciation) over time, price/earnings ratios, and so
forth.  The standardized metrics have the advantages of being objective and
supporting baseline comparisons.  However, the annual reports and other
sources of subjective information about the organization provide an
important balance to the raw metrics which can put them into perspective,
and even lead the investor to think in terms of entirely different metrics
for evaluating that organization.

If you used only annual reports to manage your financial portfolio, then you might
never sell a stock that you currently own, and you might never by any stocks
other than those you currently own.  For many reasons, most of them
quite reasonable, annual reports tend to be optimistic.  So, this is a
suboptimal strategy.  On the other hand, if you used only standard metrics
like short-term share price change to manage your portfolio, then you
might end up with the worst-case scenario for financial portfolio
management: buying at the top and selling at the bottom.

Thus, the best approach for managing a financial portfolio is to gather
and integrate together a variety of types of information, both subjective and objective.

In the case of a software portfolio, the equivalent of the "annual report"
is typically already available: it is the monthly or
quarterly progress reports provided by the individual projects regarding
their current status and future directions. The goal of SPPM
is to integrate this subjective information with
an additional, objective, complementary source of information to senior
management.  Such information is analogous to standardized financial
metrics like price/earnings ratios.

## 1.2 Benefits of SPPM for management ##

Just as a financial analyst combines together subjective and objective
information to gain insight into the portfolio, managers in a company with
a large number of projects can combine together subjective and objective
information to gain insight into how individual projects, as well as the
portfolio as a whole, is performing.  Specifically, project managers can learn:
  * Baseline process and product measures for the company as a whole. For example, over 200 Java projects, what is the median software structural quality (as measured by combining coverage, complexity, and coupling measures)?
  * Normal and abnormal variation in process and product measures.  For example, what is a "typical" number of build failures for a project in a month?  Which projects are failing their builds significantly less than average?  Which are failing their builds significantly more?
  * Organizational improvement.  How do our baselines for Q2 2009 compare to our baselines for Q2 2008? Are we as a company getting more efficient and effective at software development?


## 1.3 Benefits of SPPM for developers ##

SPPM provides important benefits for
developers as well.  Developers wish to work more efficiently and
effectively.  One way they do this is by learning from their current and
prior professional experiences about what works and what doesn't work.  A
second way is through reading technical publications which contain research
and case studies on new tools, techniques, and practices.  However, these
articles often do not contain sufficient detail for developers to
understand how to apply the practices to their own circumstances, or
whether the benefits claimed for the organization in the case study would
occur in their own organization, which might differ in significant ways.

Software project portfolio management provides a new way for developers to
learn how to work more efficiently and effectively, because it provides
them with new visibility into the projects within their own organization.
First, it enables developers to become more easily aware of projects that
are similar to their own, but that might differ along one or more objective
or subjective dimensions.  Such projects provide a valuable opportunity to
developers in both projects to gain insight into what is causing the
differences between projects in the same organization that are otherwise
similar.   Specifically, developers can learn:
  * Are there other projects in the company like ours that we are not aware of?
  * Are the trends in our project over time similar to the trends in other projects?
  * Are there other projects that have already acquired expertise with specific tools or techniques that we are just now adopting?

## 1.4 Making it practical ##

To be feasible and effective at the scale of hundreds of projects, SPPM
data collection and analysis must be as automated as possible and incur
little to no chronic overhead on developers or project teams.  The most
reasonable way to accomplish this is by restricting data collection
activities to what can be obtained via the continuous integration build
process, the source code control system, and the issue management system.

The benefit to this restriction is that the data collection process is
almost entirely transparent to the development team.

The cost of this approach is that certain kinds of data cannot be
collected.  For example, it is possible to instrument the individual
developer's editing tool (such as Eclipse or Visual Studio) in order to
detect when and whether the individual is practicing Test-Driven Design
techniques.  To obtain the "global" perspective on the organization's
software project portfolio, it is necessary to trade-off some of the more
detailed data analyses possible by intruding on the individual developer's
environment.

Given this restriction, what kinds of data can actually be collected?  The following table presents an ininitial list, organized by the source of the information.  "Build" refers to a continuous integration build tool such as [CruiseControl](http://cruisecontrol.sourceforge.net/) or [Hudson](https://hudson.dev.java.net/), "CM" refers to a configuration management tool such as [Subversion](http://subversion.tigris.org/) or [Perforce](http://www.perforce.com/), and "Issues" refers to an issue management system such as [Jira](http://www.atlassian.com/software/jira/) or [Bugzilla](http://www.bugzilla.org/).

| **Source**  | **Data**  | **Description/Purpose** |
|:------------|:----------|:------------------------|
| Build       | Code Size and Type  | The overall size of the system in LOC, as well as size of constituent source code types (Java, C++, Ruby, JSP, C#, ASP, etc.) provides valuable contextual information about the project.  This helps identify similar projects and supports baselines for subsets of the overall portfolio.  For example, Java complexity might have a different baseline than Ruby complexity. Code size and type can be automatically collected for a wide variety of languages using the open source tool [SCLC](http://code.google.com/p/sclc/). |
| Build       | Complexity | Source code complexity measures like [cyclomatic complexity](http://en.wikipedia.org/wiki/Cyclomatic_complexity) provide an indicator of how difficult a module is to understand, test, and enhance. Cyclomatic complexity can be determined for Java code using the open source tool [JavaNCSS](http://www.kclee.com/clemens/java/javancss/).  |
| Build       | Coupling  | [Software coupling](http://en.wikipedia.org/wiki/Coupling_(computer_science)) measures the dependencies among program modules.  Like complexity, coupling provides an indicator of how difficult a module will be to understand, test, and enhance. However, while complexity focuses on internal structure, coupling focuses on external structure. [DependencyFinder](http://depfind.sourceforge.net/) is an open source tool for obtaining coupling information. |
| Build       | Coverage  | [Code coverage](http://en.wikipedia.org/wiki/Code_coverage) measures the degree to which source code is exercised by its tests.  Coverage provides an indicator of the quality of testing, which is itself an indicator of the readiness of the code for use, as well as the adequacy of the testing procedures in use by the development team.  [Clover](http://www.atlassian.com/software/clover/) is a commercial tool for measuring coverage in Java. |
| Build       | CI Failures | [Continuous Integration](http://en.wikipedia.org/wiki/Continuous_Integration) failures measures the average number of times per month that a project fails the continuous integration process.  This is an indicator of development stability.  A project with healthy development practices will tend to have a low number of CI build failures over the course of a month. CI failures can be measured by a sensor attached to the build tool, such at [Ant](http://ant.apache.org/). |
| CM          | Commits   | The number of commits to the configuration management system provides an indicator of project activity. Each project will tend to have a "normal" level of commits per day and/or per developer  based upon the project's overall nature and current state. Departures from that baseline level indicate changes to the project, and projects with radically different baseline levels of commits indicate projects with significantly different development procedures.  All CM tools provide a reporting interface for commit data.  |
| CM          | Churn     | Associated with each commit is "churn", which is a measure of the number of lines of code added, deleted, and changed as a result of that commit.  Like commits, churn is a measure of project activity, and has similar applications to the creation of baseline values and comparison within and between projects. All CM tools provide a reporting interface for churn data.  |
| Issues      | Closure rate | Closure rate measures the number of open issues that are closed by a project during a given period of time.  It is an indicator of both project activity and project "progress". Closure rate is difficult to apply across projects, since the value of closure rate is quite dependent upon the way issues are recorded in a project.  One project might track many very small tasks, while another project might track much larger tasks.  The first project might have a much higher closure rate than the second, but this does not necessarily indicate more forward progress than the second. Nevertheless, changes to the baseline closure rate within a project can indicate changes to the project state. All issue management tools provide a reporting interface for closure rate.  |


# 2.0 Software Project Portfolio Management Applications #

This section presents three applications of software project portfolio management:  Internal Benchmarking, a Continuous Integration ICU, and a Developer Expertise Browser.

## 2.1 Internal Benchmarking ##

A very significant strategic advantage enjoyed by both developers and
managers in companies with a large number of development projects is the
ability to perform internal benchmarking.  Internal benchmarking, simply
stated, is the use of many company projects to obtain benchmarks, rather
than relying on external "industry standards".  It is generally easier to
obtain developer buy-in for internal benchmarking since (a) all projects
are being compared within the same company, reducing the likelihood that
the comparison is unfair, and (b) if project A compares unfavorably to
project B, it is possible to investigate the differences and determine why
the projects differ along the chosen dimension, and what action to take, if
any.  This is not possible when comparing data from an internal project to
a published industry benchmark.

### 2.1.1 Collecting the raw measures ###

The SPPM approach to internal benchmarking begins with regular, automated collection of data about projects.
For this example, we will illustrate the technique with the types of data described in the table above.
Here's an example dataset for a hypothetical Project A:

![http://hackystat.googlecode.com/svn/wiki/sppm.raw.data.project.a.png](http://hackystat.googlecode.com/svn/wiki/sppm.raw.data.project.a.png)

Note that to generate this table, decisions must be made about how to
appropriately measure a concept like "Coverage" or "Coupling".  For
example, coverage tools generally calculate several different forms of
coverage, such as statement coverage and branch coverage.  Similarly,
coupling tools determine both afferent and efferent coupling, and so one
could represent coupling as the average number of afferent and efferent
couplings per module, or the number of modules exceeding a given threshold
value of coupling.  Size can be measured in LOC, function points, methods,
and so forth.  One could even track multiple measures for the same concept.

For the purpose of our example scenario, however, let us content ourselves
with just 8 measures per project, and a time frame of five weeks, which
gives us 40 values per project.  Let's now assume that we wish to manage
just 8 projects.  Even with that very small number of projects, the current
spreadsheet representation is unwieldy: it would contain 64 rows, and a
total of 320 numbers:

![http://hackystat.googlecode.com/svn/wiki/sppm.raw.data.png](http://hackystat.googlecode.com/svn/wiki/sppm.raw.data.png)

Even with just five weeks of data, eight measures, and eight projects,
there is already an overwhelming number of numbers, and it is very
difficult to draw any useful conclusions from this chart.  Imagine having
50 weeks of data, 20 measures, and 100 projects. Clearly, this
representation is not scalable nor useful.

### 2.1.2 Data compression via sparklines ###

The prior section illustrates that it is quite easy to collect an
overwhelming amount of data for very few projects in a very short period of
time.  How can we make this dataset manageable?

One thing we can do is to recognize that, in general, the most recent value
of a measure is the one that we are most interested in, and the prior data
values are useful mainly as a way of understanding how the project arrived
at that measure.  In other words, is the current value the highest value
yet recorded?  Is it the lowest?  Is the trend increasing, decreasing, or
chaotic?

Edward Tufte invented a visualization approach called [sparklines](http://en.wikipedia.org/wiki/Sparkline) which can help us reduce the information burden significantly.  Sparklines are "data-intense, design-simple, word-sized graphics".  Using them, we can simplify our table of data considerably by representing trend information as a sparkline.  Here is what our previous table becomes with trend information represented as sparklines:

![http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.data.png](http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.data.png)

This is much, much better: instead of 64 rows and 320 numbers, we have just 8 rows and 64 numbers.  Furthermore, we can now represent an arbitrary number of historical values without increasing the number of numbers displayed (additional history simply becomes additional bars in the sparklines.)

While we have compressed the data to just one row per project, the data is still rather opaque and uninterpretable. This is where we can apply internal benchmarking.

### 2.1.3 Internal benchmarking ###

To use internal benchmarking, we apply a set of evaluation rules to each
cell of the table that results in one of three colors: red, yellow, or
green.  Red indicates an underperforming value, green represents a highly
performant value, and yellow indicates a value in the middle, or an
ambiguous value.  For each measure, we will provide two evaluation rules:
one for the latest value, and one for the sparkline, or trend.

For this scenario, the evaluation rules for the latest values will generally assign green to the "best" projects in the group, such as those in the top quartile.  Since there are eight projects total, the top quartile represents the top two projects.

On the other hand, the evaluation rules for trends are generally independent of relative performance. For example, a project whose ccoverage is steadily increasing will get green for their trend, even if their current coverage is low.

Here is an example set of evaluation rules:

| **Measure**  | **Value/Sparkline** | **Evaluation Rule** |
|:-------------|:--------------------|:--------------------|
| Coverage     | Value               | Green: top quartile; Red: bottom quartile; otherwise Yellow. |
|              | Sparkline           | Green: upward or stable; Red: downward; Yellow: unstable. |
| Complexity   | Value               | Green: top quartile; Red: bottom quartile; otherwise Yellow. |
|              | Sparkline           | Green: downward or stable; Red: Upward; Yellow: unstable. |
| Coupling     | Value               | Green: top quartile; red: bottom quartile; otherwise Yellow. |
|              | Sparkline           | Green: downward or stable; Red: upward;  Yellow: unstable. |
| Churn        | Value               | Green: Middle quartiles; Yellow: top/bottom quartiles; Red: extremes |
|              | Sparkline           | Green: stable; Yellow: unstable |
| Commits      | Value               | Green: top or middle quartiles; Yellow: third quartile; Red: bottom quartile |
|              | Sparkline           | Green: upward or stable; Red: downward; Yellow: unstable. |
| CI Success %  |  Value              | Green: top quartiles; Yellow: middle quartiles; Red: bottom quartile |
|              | Sparkline           | Green: upward or stable; Red: downward; Yellow: unstable. |
| Closure      | Value               | Green: top quartiles; Yellow: middle quartiles; Red: bottom quartile |
|              | Sparkline           | Green: upward or stable; Red: downward; Yellow: unstable. |

If we apply these rules to our sample dataset and color the latest values and the sparklines appropriately, we get the following:

![http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.color.data.png](http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.color.data.png)

### 2.1.4 Interpretation ###

The addition of evaluation rules, and the subsequent coloring of current values and sparklines, makes it now possible to easily draw a number of conclusions from the data:

  * It is now quite easy to identify high performing projects and low performing projects: just look for rows with a large amount of green (or red).  It should now be easy to see that Project A is a high performing project (its row is all green), and that Project F is a low performing project (its row is almost all red, without any green).

  * Sparklines provide valuable context for interpretation of the current value.  Consider the Coverage associated with Project C.  While the value (33) is red, indicating a low coverage value relative to other projects, the trend is green, indicating that the team is steadily increasing their test coverage.  Thus, it might be prudent as a manager to simply wait and see if this positive trend continues.  On the other hand, Projects D and H, while they have substantially higher Yellow coverage (81 and 82), both have red sparklines because their coverages are steadily dropping.  As a manager, one might want to ask these projects for more information about their test practices and why coverage has been dropping for five weeks in a row.

  * As noted above, every project can achieve "green" with respect to their sparklines, because these are independent measures.  Achieving green with repect to the current value requires a project to be a top performer in its class, because the current value evaluation rules are based upon quartiles.  One could create more complex evaluation rules that incorporate absolute thresholds, such as giving a project a Green for coverage if it gets over 90% regardless of its quartile position.  By incorporating thresholds, it would be possible for the entire portfolio to attain a green status.

### 2.1.5 Providing an overall evaluation for each project ###

If one wishes, one could aggregate the colors to generate an overall color for each project as a whole.  A simple way to do this would be to provide a color to the project based upon the "average" color among the 14 colors assigned to the project (7 sparklines plus 7 current values).  Here, for example, is the result of this overall rating:

![http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.overall.data.png](http://hackystat.googlecode.com/svn/wiki/sppm.sparkline.overall.data.png)

At a glance, one can now see that we have two green projects that are presumably doing quite well, one red project that is in trouble, and the rest are somewhere in between.

### 2.1.6 Scaling internal benchmarking to large numbers of projects ###

The table shown above seems quite reasonable for 8 projects, but what about 80?  To understand how to manage dozens or hundreds of projects, it is necessary to direct your attention to the size column, which has so far appeared to be of little interest.

Metrics such as size are not evaluative in nature, in other words, there's
no such thing as a "good" size or "bad" size, and there's no such thing as a good or bad trend in
size values.  Size, and metrics like it (such as programming language, team size, application type, project age, etc.) are best used as "context variables"---they tell you interesting features of the project that do not have a value judgement attached to them.

Context variables such as size thus become quite useful for managing large
numbers of projects by providing a way to organize and filter the set of
projects under consideration.

For example, assume you have 100 projects in your portfolio.  You can use context variables to help you create subsets of projects that can be compared more meaningfully to each other.  For example:

  * All Java projects between 50,000 and 100,000 LOC.
  * All web application projects.
  * All C++ and Java projects less than 1 year old.

## 2.2 A Continuous Integration ICU ##

### 2.2.1  Overview ###

The internal benchmarking application illustrated above is designed to be
scalable to large numbers of projects. In order to accomplish that goal, a
high degree of abstraction is required.  For example, trends in metrics
over time are reduced to tiny sparkline graphics that provide only a sense
for whether the trend is increasing, decreasing, stable, or chaotic.  The
"health" of a metric, and of the project as a whole, is reduced to one of
three states: green, yellow, or red.  When attempting to obtain an overall
perspective on 50, 100, or 500 software projects, such abstractions are
unavoidable and useful.

Now, however, consider the situation where internal benchmarking has been
used to identify just a handful of projects, and now more detailed
information is desired.  You now need a way to "drill down" into the
particulars of just one or two projects at a time.

### 2.2.2 Requirements for the CI-ICU ###

A situation in which just one or two projects are the focus, rather than 100, creates very different requirements:

  * You might want to know more details about the metrics and how they were derived.  Instead of an aggregate statistic like "Overall coverage is 80%", you will want to know, for example, the distribution of coverage across classes.  How many classes had high coverage? How many had low?  Was the distribution of coverage bi-modal? Are there a few outliers that are dramatically affecting the overall percentage? This information is important to understanding the impact of the coverage value on quality. Do the two projects under consideration have different distributions, even though their aggregate value for the metric is essentially equal?

  * You might want to know whether different metrics for the same conceptual measure have different values.  For example, does branch coverage differ significantly than statement coverage?

  * You might want to know if two or more measures are co-varying.  For example, does the likelihood of build failure increase with the amount of churn associated with a commit? If so, this could indicate a causal relationship between these two measures.

To help make the information made available by internal benchmarking actionable, the next step is to provide details about the measures associated with a small number of projects at a time.  This is the goal of software project telemetry.  More details about this approach are provided in the [The Continuous Integration ICU](ContinuousIntegrationICU.md) page.  A sample scenario of how software project telemetry can reveal different characteristics of development is provided in the [Software Project Telemetry Tutorial](http://code.google.com/p/hackystat/wiki/Tutorial_SoftwareProjectTelemetry).

### 2.2.3 Example screenshots ###

Finally, here are two screen shots from the ProjectBrowser application that illustrate some of these ideas:

![http://hackystat.googlecode.com/svn/wiki/sppm.dpd.png](http://hackystat.googlecode.com/svn/wiki/sppm.dpd.png)

This screen shot shows the distribution of coverage values for a single project and day.  Clicking on a link pops up a window showing the classes in that coverage "bucket".

![http://hackystat.googlecode.com/svn/wiki/sppm.telemetry.png](http://hackystat.googlecode.com/svn/wiki/sppm.telemetry.png)

This screen shot shows trends in the number of build invocations for two projects.  As the trend lines show, the build invocations for these two projects are highly correlated.

## 2.3 Developer Expertise Browser ##

### 2.3.1 Overview ###

The prior two applications show how software project portfolio management
techniques can be used to provide new transparency into the state and
trends associated with project processes and products.

This third example, called the Java Developer Expertise Browser (JDeb),
shows a very different application of these techniques to support better
mining and visibility of developer expertise. (We will use Java for this
example, but the same approach can be applied to almost all modern languages.)

Java has a rich ecosystem of application libraries available for it which
can simplify programming in domains as diverse as web application
development, high performance computing, bioinformatics, artificial
intelligence, and so forth.  Even the core Java language itself contains
dozens of libraries for various purposes.  There are literally thousands of
widely used libraries available for Java which can substantially reduce the
time required to code new applications in a given domain.

While the availability of these libraries provides the potential for
significant productivity and quality benefits, there is a learning curve
associated with any new library, which can sometimes be substantial.
Furthermore, any given domain might have more than one library available
for it, with different strengths and weaknesses associated with them.

In an organizational environment with dozens or hundreds of Java projects
under development, there is likely to be hundreds to thousands of different
Java libraries in use across the organization.  Any given developer might
be familiar with perhaps a dozen libraries at any given time.

### 2.3.2 Requirements ###

The basic problem addressed by a JDeb is to enable developers in an organization to quickly find out who in their organization appears to have expertise with a given library.  This has a number of potential applications:

  * When evaluating a number of libraries for adoption on a project, JDeb can help a developer find out if there are developers with prior experience with any of these libraries so that they can elicit feedback;

  * If a developer is taking over a code base from a departed colleague, JDeb can help them find other developers in the organization who have expertise with a library;

  * When using a library for the first time, if an error is encountered, JDeb can help the developer find other developers who might be able to help them resolve the problem quickly.

### 2.3.3 Mining source code for library references ###

JDeb is built upon two kinds of data.  First, the compiled version of every Java source file can be examined to determine which libraries it references.  This roughly corresponds to the import statements associated with a Java class.  For example, here are the import statements associated with a single class in the DailyProjectDataClient class:

```
import java.io.StringReader;
import java.util.Date;
import java.util.logging.Logger;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.Unmarshaller;
import javax.xml.datatype.XMLGregorianCalendar;

import org.hackystat.dailyprojectdata.resource.build.jaxb.BuildDailyProjectData;
import org.hackystat.dailyprojectdata.resource.codeissue.jaxb.CodeIssueDailyProjectData;
import org.hackystat.dailyprojectdata.resource.commit.jaxb.CommitDailyProjectData;
import org.hackystat.dailyprojectdata.resource.complexity.jaxb.ComplexityDailyProjectData;
import org.hackystat.dailyprojectdata.resource.coupling.jaxb.CouplingDailyProjectData;
import org.hackystat.dailyprojectdata.resource.coverage.jaxb.CoverageDailyProjectData;
import org.hackystat.dailyprojectdata.resource.devtime.jaxb.DevTimeDailyProjectData;
import org.hackystat.dailyprojectdata.resource.filemetric.jaxb.FileMetricDailyProjectData;
import org.hackystat.dailyprojectdata.resource.unittest.jaxb.UnitTestDailyProjectData;
import org.hackystat.utilities.logger.HackystatLogger;
import org.hackystat.utilities.tstamp.Tstamp;
import org.hackystat.utilities.uricache.NewUriCache;

import org.restlet.Client;
import org.restlet.data.ChallengeResponse;
import org.restlet.data.ChallengeScheme;
import org.restlet.data.MediaType;
import org.restlet.data.Method;
import org.restlet.data.Preference;
import org.restlet.data.Protocol;
import org.restlet.data.Reference;
import org.restlet.data.Request;
import org.restlet.data.Response;
import org.restlet.data.Status;
import org.restlet.resource.Representation;
```

Ignoring the imports from within the hackystat project itself for a moment, you can see that the DailyProjectDataClient class imports 18 classes from external libraries.  Because of the hierarchical nature of the package naming system, you can see that DailyProjectDataClient uses three "major" libraries:  java, javax, and org.  Going down one level, you can now see that it uses four libraries: java.io, java.util, javax.xml, and org.restlet.  Each step down the package hierarchy provides more specific information about the libraries in use.

Thus, by simply collecting the package import data associated with Java source code, we can find out about the libraries used by a given source code file.  Tools such as [DependencyFinder](http://depfind.sourceforge.net/) make it quite simple to collect this information automatically.

### 2.3.4  Commit data as a proxy for developer expertise ###

To make JDeb useful, we need to know more than what libraries are used by
the Java software in the organization, we need to also know who is familiar
with which library.  JDeb infers this information from the commit history
associated with a file.  If a developer has committed changes to a file,
then they are presumed to have at least some familiarity with the libraries
associated with that file.  The more commits they have made to a file, the
more familiarity they are assumed to have.  JDeb also takes into account
the recency of the commits, so that if a developer made commits to a file a
year ago, then the developer might have forgotten details about that
library since then.

Commit information can also be collected automatically from the configuration management system.

### 2.3.5  Example use ###

The use of JDeb is simple and intuitive. Assume that JDeb has been running for six months, and has regularly collected import information from files, along with the developers who have committed to those files.  It has thus accumulated a knowledge base of information about what libraries are in use by the organization, as well as who has experience with those libraries.

Now a developer has discovered a bug in their program related to the org.apache.wicket.protocol.http.WebApplication class. They bring up the JDeb interface and search on "org.apache.wicket".

![http://hackystat.googlecode.com/svn/wiki/sppm.jdeb.1.png](http://hackystat.googlecode.com/svn/wiki/sppm.jdeb.1.png)

This result is encouraging: there are three developers in JDeb's knowledge base with experience with the org.apache.wicket library.
The developer might at this point simply contact those three developers to ask a question.  Or, the developer might refine the search by seeing who has experience with the protocol sublibrary.  JDeb might respond as follows:

![http://hackystat.googlecode.com/svn/wiki/sppm.jdeb.2.png](http://hackystat.googlecode.com/svn/wiki/sppm.jdeb.2.png)

As you can see, with further refinement in the library specification, it is now clear that the developer senin@hawaii.edu might be the best person to contact initially.

There are many refinements that can be made to this initial approach.  One can become more sophisticated in the way one specifies expertise and take into account factors other than simple commits.  One can enable users to manually adjust their automatically generated expertise profile to better reflect their skill sets.  The user interface can be made more sophisticated, such as by supporting a way to browse a package hierarchy and see the users with expertise at each level.

The core concept remains the same, however. In a large organization, developers are accumulating expertise with technologies every day.  This expertise is largely hidden, and largely unavailable to the community as a whole.  By capturing this expertise automatically, the organization can makes its own intellectual resources available to itself more easily and more effectively.

