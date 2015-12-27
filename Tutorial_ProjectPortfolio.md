## 1.0 Motivation ##

If you read the [Guided Tour](http://code.google.com/p/hackystat/wiki/Tutorial_GuidedTour), you will know that Hackystat is a system in which users install sensors, collect raw data, define projects, and then run analyses to better understand the state of their project(s), potential issues, and potential opportunities for improvement.

For some organizations, the problem isn't simply understanding and assessing a single project, it's understanding and assessing dozens or hundreds of projects.  In these situations, developers and managers must deal with a  "portfolio" of projects.  Organizations with a portfolio of projects face interesting challenges as well as opportunities.  The challenges involve collecting, analysing, and utilizing data across projects with different characteristics and contexts with acceptable cost/benefits.  The opportunities involve the ability for project groups to more effectively gain insight from each other.  For example, one team might be experimenting with a new development tool or technique that dramatically increases their test coverage.   In traditional organizations, leveraging this advance requires information "push":  the team must allocate time and resources to publicizing their advance to the entire organization, on the chance that another team might be interested in their results at that time.   In organizations with an effective portfolio-level analysis mechanism, the analysis can help change information "push" into information "pull":  teams that are interested in high coverage can use the portfolio analysis to find other teams that are succeeding, and then query them for details.

To investigate effective support for organizations with many projects, Hackystat provides Software Project Portfolio Analysis (SPPA).  It is designed to address two primary goals:

  * Scalability:  portfolio analysis should enable visualization and comparison of dozens to hundreds of projects.
  * Evaluation: portfolio analysis should embody rules that help developers understand the results by assigning, when possible, "positive" or "negative" interpretations to the data.

We are just starting the evaluation of SPPA, and would enjoy hearing from organizations who wish to help us with the enhancement of this capability.

## 2.0 A sample portfolio analysis ##

SPPA is best introduced with a screen image showing a sample analysis on three hypothetical projects with simulated data:

![http://hackystat.googlecode.com/svn/wiki/projectportfolio.1.gif](http://hackystat.googlecode.com/svn/wiki/projectportfolio.1.gif)

On the left hand side, you can see a selection window in which the user has selected the dates they are interested in, the "granularity" (days, weeks, or months), and the projects upon which they wish to run SPPA.  In this example, three projects have been selected.  In addition, there is a link to the Configuration screen, which allows customization of the portfolio analysis.  We will show that window later.

On the right hand side, the black table shows the results of running SPPA on the three projects.  Each project's data is displayed in a single row.  Columns represent different forms of process and product data:  Coverage, Complexity, etc.

For each type of process/product data, SPPA provides two kinds of information:  a Sparkline histogram showing the trend in that data over time, and a number showing its most recent value.  For example, in the GoodProject, Coverage is rising slightly over time, and its most recent value is 93.

Finally, SPPA displays the data using one of four colors:  red, yellow, green, or white.  Red means "bad", green means "good", yellow means "unstable", and white means "no interpretation for this kind of data".   Trends and most recent values are colored independently.   For example, when analyzing Coverage, an increasing trend is "good" and a decreasing trend is "bad".  When analyzing Complexity, on the other hand, a decreasing trend is "good" and an increasing trend is "bad".  Finally, Size trends have no general interpretation: increasing is not necessarily good, and decreasing is not necessarily bad, nor vice-versa.

While some data values have no interpretation, they can provide useful contextual information.  For example, you can see that the size of UnstableProject is an order of magnitude larger than the other projects. One should take care when comparing a project of 30,000 LOC to a project with 600,000 LOC.

The most recent values are colored according to thresholds. An organization might decide that coverage over 80% is "good", coverage under 50% is "bad", leaving anything in between as "unstable".

With this in mind, you can hopefully see why the hypothetical projects have their names.  Looking at the GoodProject row, almost all of the data values with interpretations are green and there are no red values.  Conversely, the TroubledProject row has almost all red values with no green.  The UnstableProject is mostly yellow.

Independently evaluating the trend and most recent values can provide useful insight.  For example, consider the Coverage data for TroubledProject and UnstableProject.  Both projects have almost the exact same current value (73.0 and 74.0).  This is an intermediate level of coverage, and so is colored yellow for both projects.  However, the trends are different:  in the case of TroubledProject, 73.0 represents the result of a steadily falling trend in coverage, and so is colored red.  On the other hand, UnstableProject has been steadily increasing its coverage, and 74.0 represents it's best coverage yet.  Thus, its trend line is green.  Thus, while both projects have the same current level of coverage, one would expect that UnstableProject's current coverage might continue to improve and become green, while TroubledProject's current coverage might continue to decline and become red.

## 3.0 Configuration ##

For SPPA to be generally useful, it must be possible for an organization, or even an individual user, to configure the analysis.  The SPPA configuration screen provides this capability:

![http://hackystat.googlecode.com/svn/wiki/projectportfolio.2.gif](http://hackystat.googlecode.com/svn/wiki/projectportfolio.2.gif)

Using the configuration screen, one can tailor the portfolio analysis with the following:

  * **Enabled:**  This checkbox indicates which analyses are to be displayed as columns in the table.  Note that any telemetry chart that returns a single stream is eligible to be a portfolio column.  Instructions on how to enhance your Hackystat installation with site-specific analyses based upon your own telemetry charts is forthcoming.

  * **Colorable:** This checkbox indicates whether or not the analysis should be colored red/yellow/green.  If the analysis is not colorable, then it will appear in white.

  * **Is higher better:**  This checkbox indicates whether rising trends in the analysis value are good.  For example, this is checked in the case of Coverage, and unchecked in the case of Coupling.

  * **Higher threshold and lower threshold:**  These two fields take two numeric values which separate the most recent data values into three regions: above the higher threshold, below the lower threshold, and in between the two.  If "is higher better" is checked, then values above the higher threshold will be green, the middle will be yellow, and below the lower threshold will be red.

  * **Telemetry parameters:**  Each analysis, which corresponds to a single telemetry chart, can also have one or more optional parameters that allow customization of the telemetry trends returned.



## 4.0 Telemetry drill down ##

Because the Sparkline is based upon telemetry, you can click on any Sparkline to retrieve the corresponding telemetry chart.   For example, if you click on the Coverage Sparkline associated with the GoodProject, the following Telemetry chart will be displayed in the Telemetry page of the ProjectBrowser.

![http://hackystat.googlecode.com/svn/wiki/projectportfolio.3.gif](http://hackystat.googlecode.com/svn/wiki/projectportfolio.3.gif)


## 5.0 For more information ##

Please post your query to the hackystat-dev mailing list or send comments to Philip Johnson (johnson@hawaii.edu)
