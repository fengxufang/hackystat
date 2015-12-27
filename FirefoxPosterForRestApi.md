## 1.0 Motivation ##

Several Hackystat services provides REST APIs, such as the [SensorBase REST API](http://code.google.com/p/hackystat-sensorbase-uh/wiki/RestApiSpecification), the [DailyProjectData REST API](http://code.google.com/p/hackystat-analysis-dailyprojectdata/wiki/RestApiSpecification), and the [Telemetry REST API](http://code.google.com/p/hackystat-analysis-telemetry/wiki/RestApiSpecification).  These services return XML representations of Hackystat data and analyses, which are then transformed into more user-friendly HTML representations by such user interface services as the ProjectBrowser or TelemetryViewer.

In many situations, such as writing new services or clients, it is useful to become acquainted with the REST API directly, and learn how to send and receive XML data with a given service.  To do this, one could write a simple program such as a Java application that uses the various Java-based client classes, but a much simpler and faster approach is to use the [Poster plugin for Firefox](https://addons.mozilla.org/en-US/firefox/addon/2691). The advantages to using Poster are:

  * You can interact directly with the service and see what data is returned without any "prettification" by the browser.
  * You can use all HTTP operations: POST, PUT, DELETE, as well as the GET operation supported by the browser address bar.
  * You can easily specify authentication credentials.

The goal of this page is to simply show example invocations of the Poster plugin for Hackystat services and the return values. This will hopefully make it easier for users to get started using Poster.

## 2.0 SensorBase ##

A query to the sensorbase service:

![http://hackystat.googlecode.com/svn/wiki/poster.sensorbase.gif](http://hackystat.googlecode.com/svn/wiki/poster.sensorbase.gif)

After pressing the "GET" button, the response is:

![http://hackystat.googlecode.com/svn/wiki/poster.sensorbase.response.gif](http://hackystat.googlecode.com/svn/wiki/poster.sensorbase.response.gif)

## 3.0 DailyProjectData ##

A query to the dailyprojectdata service:

![http://hackystat.googlecode.com/svn/wiki/poster.dailyprojectdata.gif](http://hackystat.googlecode.com/svn/wiki/poster.dailyprojectdata.gif)

After pressing the "GET" button, the response is:

![http://hackystat.googlecode.com/svn/wiki/poster.dailyprojectdata.response.gif](http://hackystat.googlecode.com/svn/wiki/poster.dailyprojectdata.response.gif)


## 4.0 Telemetry ##

A query to the telemetry service:

![http://hackystat.googlecode.com/svn/wiki/poster.telemetry.gif](http://hackystat.googlecode.com/svn/wiki/poster.telemetry.gif)

After pressing the "GET" button, the response is:

![http://hackystat.googlecode.com/svn/wiki/poster.telemetry.response.gif](http://hackystat.googlecode.com/svn/wiki/poster.telemetry.response.gif)

