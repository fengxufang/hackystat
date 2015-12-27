This page provides a step-by-step guide for a demo of Hackystat. It involves logging in to the public
ProjectBrowser service with a test account and running various analyses, as described in the following sections:




## Bring up the ProjectBrowser home page ##

To access the public instance of the ProjectBrowser service, go to [http://dasha.ics.hawaii.edu:9879/projectbrowser/](http://dasha.ics.hawaii.edu:9879/projectbrowser/).

You should see the following page:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.gif)

## Login as joe.simpleportfolio@hackystat.org ##

Next, supply "joe.simpleportfolio@hackystat.org" as both the username and the password, as illustrated here:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.2.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.2.gif)

Upon successful login, you should see the SensorData page:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.3.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.3.gif)

## Display sensor data for July 2008 ##

This test user has sensor data (stored in the SensorBase service) for July of 2008.  To retrieve and display a summary of the raw sensor data for
July, select July 2008 and press submit. You can leave the project specified as "Default".  You should see the following page:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.4.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.4.gif)

This page shows, for each day of the specified month, the types of sensor data (Build, Code Issue, Commit, Coupling, etc.) stored in the SensorBase and how many instances of each type of data were sent by a given tool (Ant, FindBugs, Subversion, etc.) for the given day, user, and project.

If you click on a link, such as "9:Ant" for the first day of the month, the ProjectBrowser service will request detailed information from the SensorBase regarding the 9 sensor data instances sent by the Ant tool on that day and pop up a modal window with the information:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.5.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.5.gif)

This illustrates inter-service communication between the ProjectBrowser service and the SensorBase service using RESTful http calls.  If you are interested in more details, you can view the [SensorBase REST API specification](http://code.google.com/p/hackystat-sensorbase-uh/wiki/RestApiSpecification).

## Display Daily Project Data ##

Raw sensor data is generally too fine-grained to make use of, so Hackystat provides another service called DailyProjectData which produces higher-level abstractions of the raw sensor data for a given project and day.  The ProjectBrowser can communicate with this service using the DailyProjectData page, which you retrieve by clicking on the associated tab in the menu bar.

Once there, you can retrieve a variety of DPD analyses, depending upon the sensor data associated with the user.  To see an example analysis, select "2008-07-01" as the date, select the Good, Troubled, and Unstable projects, "Coverage" as the analysis, "Count" as the Value, and "Line" as the coverage type.  Then press submit, and you should see the following:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.6.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.6.gif)

To produce this page, the ProjectBrowser service makes a RESTful request of the DailyProjectData service for Coverage information on three projects.  This results in the DailyProjectData service making requests of the SensorBase service for the raw sensor data, which it processes in order to return the requested analysis.

As you can see from the screen, the three sample projects each have only a single class associated with them, but the classes all differ in their level of test coverage. The Good Project class has coverage between 60% and 80%, the Troubled Project has coverage between  80% and 100%, and the Bad Project has coverage between 40% and 60%.  (It may seem strange the the Troubled Project has the highest coverage: you will see why in a minute.)

If you are interested in more details, you can see the  [REST API for the DailyProjectData service](http://code.google.com/p/hackystat-analysis-dailyprojectdata/wiki/RestApiSpecification).

## Display Telemetry Data ##

The DailyProjectData service provides a more useful abstraction than the raw sensor data, but in many cases it is useful to see the trends in this data over time.  To support that, Hackystat provides the Telemetry service. As usual, the ProjectBrowser provides an interface to the Telemetry service using the Telemetry page.

To see the Telemetry service in action, go to the Telemetry page, select "2008-07-01" as the From Date, "2008-07-15" as the To Date, select "Day" as the Granularity, check our three projects (Good, Troubled, Unstable), select the "Coverage" chart, "Percentage" as the mode, and type in "line" as the granularity.  Then click on "Display Telemetry", and you should see the following:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.7.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.7.gif)

This analysis involved the ProjectBrowser service making requests to the Telemetry service, which in turn makes requests to the DailyProjectData service, which in turn makes requests to the SensorBase service.  As detailed in the IEEE Service Cup paper submission, Hackystat implements multiple forms of caching to avoid the potential overhead of such chained requests.

Now you can create a chart using the [Google Chart service](http://code.google.com/apis/chart/).  In the displayed table, select the three projects, and click "Get Chart".  The ProjectBrowser will send a request to the Google Chart service with the data points to chart, and display the returned image:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.8.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.8.gif)

If you are interested in more details, you can see the [REST API for the Telemetry service](http://code.google.com/p/hackystat-analysis-telemetry/wiki/RestApiSpecification).

## Display Portfolio data ##

The final stop on this brief introduction to Hackystat services is the Portfolio page.  This page presents a high level summary of DailyProjectData and Telemetry service information, along with interpretation rules to color the data red, yellow, or green depending upon whether the associated data appears "healthy", "unstable", or "unhealthy".

To see the Portfolio analysis in action, go to the Portfolio page by clicking on the associated tab, then enter "2008-07-01" as the From Date, "2008-07-31" as the To Date, "Week" as the granularity, and select our three projects.  Finally, click "OK" at the bottom of the page to generate the portfolio analysis, which should look like this:

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.9.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.pb.9.gif)

An explanation of Portfolio analysis goes beyond the scope of this demo; for details you can read the [Portfolio Tutorial](http://code.google.com/p/hackystat/wiki/Tutorial_ProjectPortfolio).  What you can see from this image is that a great many Telemetry trends (as represented by the sparkline histograms) and DailyProjectData instances (as represented by the numeric values) are being retrieved.  Note that the Portfolio analysis needs some custom configuration in order to display all of the demo data, which is why (for example) Coverage data does not appear. (Custom configuration goes beyond the scope of this simple demo.)

If you look at the three rows, you can see that the Good project is colored green, Troubled project is colored mostly red, and the Unstable project is colored mostly yellow.

## Thanks for your interest ##

We hope you enjoyed this quick demo of Hackystat services.  If you have any questions, feel free to contact Philip Johnson (johnson@hawaii.edu).  The following chart provides a quick review of the services used in this demo. The arrows indicate the general direction of data flow.

![http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.arch.gif](http://hackystat.googlecode.com/svn/wiki/IEEEServiceCupReview.arch.gif)

Note that this demo does not include information about how sensor data gets into the system, nor about other Hackystat services such as Tickertape.







