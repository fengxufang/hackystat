| ![http://hackystat.googlecode.com/svn/wiki/ICU.jpg](http://hackystat.googlecode.com/svn/wiki/ICU.jpg) |
|:------------------------------------------------------------------------------------------------------|
| An example of human "telemetry" in an ICU                                                             |

## 1.0 Introduction ##

In medicine, the "ICU", or intensive care unit, is a place designed to
support continuous monitoring of a person's vital signs, such as heart
rate, blood pressure, respiration, EKG, ECG, and so forth.  Each of these
vital signs provides only a limited perspective on the health of the
person, and what is "normal" for one person in a given situation might be
quite abnormal for another person in another situation.

Despite these limitations, continuous monitoring of a variety of vital
signs can support fast and effective responses to emergent conditions
before the person being monitored undergoes significant harm.

By continuously monitoring multiple, independent measures of "health", one
can quickly establish a baseline for what is "normal" for a given person or
population.  In addition, multiple independent vital signs, all changing in
concert, provide important information about the severity of a condition
and can also provide some indication of how or when to respond.

Note that continuous monitoring of vital signs is not restricted to sick
people.  Astronauts, fighter pilots, and other people in extreme conditions
and environments often have many of their vital signs under continuous
monitoring.

This document outlines how the Software Project Telemetry capabilities in
Hackystat can be used to construct a kind of intensive care unit for
projects under continuous integration control. We call this system approach the  "CI-ICU" for short.
Constructing a CI-ICU for your software projects is intended to provide the
following benefits:

  * An understanding (i.e. a "baseline") for what constitutes a healthy project's vital signs.

  * An understanding of "natural variation" in healthy project vital signs for a given project, and across all projects.

  * An early warning system for projects in trouble.

  * Additional data for project post-mortems.  If the vital signs indicated health but the project died, what went wrong?  What additional vital signs could have saved the patient, or did it die due to "natural causes"?

The next section details a set of telemetry streams that form a potentially
interesting set of "project vital signs".  Note that this set of streams is
a subset of those possible with Hackystat.  We limit ourselves to streams
that can be obtained from a continuous integration system, and omit
telemetry data that requires installing sensors into each developer's
environment.  For example, while it might be quite helpful to know if
developers are practicing test-driven design, the CI-ICU cannot monitor
that because the required sensor data cannot be obtained from the
continuous integration system. Monitoring TDD requires sensors to be
installed into the developer IDE, which is outside the scope of the CI-ICU.
To stretch the metaphor, the CI-ICU is intended to be "non-invasive" and
thus not require detailed monitoring of individual developer behaviors.

## 2.0 CI-ICU Telemetry Streams ##

Each of the following sections briefly describes a possible telemetry
stream and how it can be used as a partial indicator of project "health".

### 2.1 Builds ###

Description: Build sensor data provides information about the frequency of build events
during continuous integration, as well as the success or failure of those
builds.

Example tools: Ant, Make.

Potential telemetry streams:
  * Total number of builds
  * Number of successful builds
  * Number of failing builds
  * Percentage successful builds

Indicators of health:
  * Frequency of build events during a day should fall with a baseline range.
  * Frequency of build failures should be low.

Potential symptoms of problems:
  * An abnormal number of build events for a given project indicates an abnormal development pattern.
  * A project with a baseline number of builds that are abnormally low or abnormally high compared to other projects may indicate a different development process.
  * An abnormally high number of build failures indicates a development team making commits that are not appropriate for the CI system.

### 2.2 Unit Tests ###

Description: UnitTest sensor data provides information about the number of unit tests invoked during a CI build, and the number of those unit tests that passed or failed.

Example tools: JUnit.

Potential telemetry streams:
  * Total number of unit test invocations.
  * Number of successful tests.
  * Number of failing tests.
  * Percentage of successful tests.

Indicators of health:
  * The absolute number of unit tests invoked by a given project is steady or increasing.
  * The density of unit tests (number of unit tests invoked per KLOC of project code) should be stable or increasing.
  * Frequency of unit test failures should be low.

Potential symptoms of problems:
  * Decreases in the number (or density) of unit tests invoked during a CI build indicates potentially less attention to quality assurance activities.
  * Increases in the number of unit test failures during CI indicates code not ready for commit to CI.

### 2.3 Coverage ###

Description: Coverage sensor data provides information about the amount of production code that is exercised by the test code.

Example tools: Emma, Clover.

Potential telemetry streams:
  * Statement coverage
  * Branch coverage
  * Method coverage

Indicators of health:
  * Coverage should be high or increasing over time.  Statement coverage of over 80% is not unreasonable.

Potential symptoms of problems:
  * Decreasing coverage indicates new code being added without sufficient testing.  Note that coverage is one of the most easily misused and misinterpreted quality assurance metrics.  See [How to misuse Code Coverage](http://www.testing.com/writings/coverage.pdf) for details.


### 2.4 Complexity ###

Description: Complexity sensor data provides information about the complexity of source code using a metric like [Halstead's Complexity](http://en.wikipedia.org/wiki/Halstead_Complexity_Measures) or [McCabe's Cyclometric Complexity](http://en.wikipedia.org/wiki/Cyclomatic_complexity).

Example tools: JavaNCSS.

Potential telemetry streams "templates":
  * Average complexity
  * Total methods above a threshold value.

These two stream templates can be instantiated with:
  * A given complexity measure (Cyclomatic Complexity, Halstead Complexity)
  * "Regular" or "Normalized" complexity.  Normalized complexity divides the complexity value for a method by the size of the method.

Thus, if you are generating sensor data on two complexity measures, there are eight potential telemetry streams.

Indicators of health:
  * The average complexity over all methods in the system should be stable and low.
  * The number of methods over a given threshold (such as Cyclomatic Complexity > 10) should be stable and low.

Potential symptoms of problems:
  * An increasing average complexity and/or increasing numbers of methods exceeding a theshold may indicate more errors and/or more difficulties in testing.

### 2.5 Coupling ###

Description: Coupling (or Dependency) sensor data provides information about the degree of interdependence between program modules.  In general, program designs should strive for low coupling (modules are relatively independent of each other) and high cohesion (with a module, each of its constituents are highly related to each other).  High coupling is associated with ripple effects during changes, low readability, low reuse capability, and difficulties in testing.

Example tools: JDepend.

Potential telemetry streams:
  * Afferent coupling
  * Efferent coupling
  * Afferent + Efferent coupling
  * Dependency cycles

Indicators of health:
  * The average level of package or class-level coupling should be stable and low.
  * There should be no dependency cycles.

Potential symptoms of problems:
  * A high level of coupling, or an increasing level of coupling, may indicate increased maintenance and/or development costs.
  * High coupling relative to other similar projects may indicate a complex design and/or possibility for refactoring.

### 2.6 Commits ###

Description: Commit sensor data provides information on the frequency of commits to the continuous integration system.  This is normally highly correlated with the build sensor data, although multiple commits, close in time, will normally result in only a single build.

Example tools: SVN, CVS, Perforce.

Potential telemetry streams:
  * Number of commits
  * Average commits per day over last seven days

Indicators of health:
  * The project has a regular and consistent number of commits.

Potential symptoms of problems:
  * A drop off in commits can indicate that implementation progress has been stalled.
  * A sudden surge of commits can indicate a project in "death march" mode.
  * Long intervals between commits, in concert with high churn, can indicate that work should be decomposed into smaller chunks in order to facilitate integration.


### 2.7 Churn ###

Description: Churn sensor data indicates the number of lines of code added, changed, or deleted as a result of a commit.

Example tools: SVN, CVS, Perforce

Potential telemetry streams:
  * Total number of lines added, changed, and deleted.
  * Average number of lines added, changed, and deleted per day over last seven days.

Indicators of health:
  * In general, churn stays below some threshold level.

Potential symptoms of problems:
  * A spike in churn can indicate that a project is attempting to do too much.  However, refactoring tools can often lead to a spike in churn without indicating any project problems.

### 2.8 Issues ###

Description: Issue sensor data indicates the number of outstanding issues associated with a project, and/or the length of time an issue is open before it is closed.

Example tools: Jira, Bugzilla.

Potential telemetry streams:
  * Number of outstanding issues.
  * Average length of time to close an issue.
  * Number of open issues whose open time is above a threshold value.

Indicators of health:
  * The number of outstanding issues is relatively low and stable.
  * The average length of time that an outstanding issue is open is stable.

Potential symptoms of problems:
  * The number of outstanding issues is steadily rising.
  * The average length of time that outstanding issues are open is steadily increasing.


### 2.9 Size ###

Description: Size sensor data indicates the number of lines of code in the system for each language type.  Size data within an individual project is not generally much of an indicator of project health.  Size data can be quite useful in figuring out whether two project "patients" are morphologically similar to each other and thus their other vital signs can be compared.  A project consisting of 2000 LOC of Java will typically have quite different vital signs compared to a 2 MLOC project written in six different languages.

Example tools: SCLC.

Potential telemetry streams:
  * Size of system, per language type.  Thus, a system implemented in three languages will have three size telemetry streams.

Indicators of health:
  * Size that shows "gradual" changes, either upwards or downwards.

Potential symptoms of problems:
  * Sudden, dramatic changes in size.
  * No change at all in size.  (He's dead, Jim.)

### 2.10 FieldFaults ###

Description: Some development projects include a rapid release cycle: a version is built in Continuous Integration, then deployed into the field within hours or days.  In such cases, it can be useful to automatically monitor "field faults" from the deployed application and generate a trend line.  Field faults are highly application dependent.  Examples include: exceptions thrown, restarts, heap usage over some threshold, workstations being thrown over cubicle walls, etc.

Example tools: Java Service Wrapper (for restarts).

Potential telemetry streams:
  * Number of field faults

Indicators of health:
  * A stable, low number of field faults.

Potential symptoms of problems:
  * Highly variable field faults.
  * Increasing number over time.


## 3.0 Making it actionable ##

### 3.1 Adding contextual information ###

In a medical ICU, a patient that suddenly spikes a high fever will produce
a corresponding "telemetry event" in the ICU monitoring equipment that will
bring medical attention.  The ultimate response could be anything from
providing ibuprofen, to performing emergency surgery, to doing nothing at
all and letting the body take care of the fever by itself.  The telemetry
signal--body temperature--provides only a very coarse indication of
abnormality; it is up to the medical personnel to use additional context
and diagnostic reasoning to determine what action to take based upon this
information.

So it is with the CI-ICU: the 30 or so telemetry streams indicated above
will not, by themselves, tell you exactly what to do in response to the
data.  Put another way, they do not do diagnosis.  Instead, just like
medical monitoring equipment, they are intended to provide information that
a trained medical person can use, in concert with other information, to
distinguish "normal" from "abnormal".  Medical personnel do not rely solely
on monitoring devices for assessment, they also look at the patient, talk
with them, and perhaps gather information from friends and family.
Similarly, the CI-ICU should be viewed as a useful component of a larger
information gathering strategy to assess project health.

### 3.2 Product vs. process telemetry ###

To understand how to react to CI-ICU data, it helps to partition the set of
telemetry streams into two broad categories: product and process.

Complexity, coupling, coverage, and size are all examples of "product"
telemetry: they tell you something about the state of the code base.  If
complexity and coupling are going up, and coverage is going down, then you
have three orthogonal measures related to software product quality that all
show negative trends simultaneously.

Unit test invocations, churn, issues, commits, and builds are examples of
"process" telemetry: they provide information about how the software is
being built.  A system could have low average complexity, low average
coupling, and 100% statement coverage, but if there there are no commits or
churn then development is effectively "dead".

It is typically most useful to gather one "gestalt" for the product
telemetry, and another for the process telemetry, and then see if they
interrelate. For example, a project might be cruising along with regular
and stable product and process telemetry, and then all of a sudden there is
a spike in churn (process) that is accompanied by a sudden dip in coverage
(product).  Such a situation might not warrant much, if any, action beyond
continued monitoring. In many cases, this aberration is self-correcting and
both process and product telemetry will return to normal levels.

### 3.3 Drilling down ###

In a medical ICU, different patients have different kinds of monitoring,
and the kinds of monitoring they have may change over time according to
their condition and needs.  Similarly, the telemetry streams identified
above serve only as a starting point.

For some projects in some organizations, the value of telemetry can be
increased significantly by adding more "invasive" measures, such as
development time, measurement of use of specific practices such as test
driven design, local testing and commit behaviors, and so forth.


### 3.4 From ICU to epidemiology ###

Epidemiology is the study of the incidence, distribution, and control of
disease in a population.  For large organizations that have a population of
software projects under development, the CI-ICU can provide a means to
apply epidemiological principles to the management of their software
project portfolios.

In this situation, statistical analyses can be used to develop profiles of
"normal" telemetry based upon samples across the population.  One can look
for "outbreaks" of unhealthy telemetry trends, which might be an indication
of high level management problems.

Interestingly, software project epidemiology provides a new kind of
"context" for understanding the telemetry data for any given project; a
context based upon how other projects measured in similar situations.



















