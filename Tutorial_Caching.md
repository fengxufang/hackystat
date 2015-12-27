## Executive Summary ##

> _Hackystat implements caching to improve performance.  Cached values can become invalid if you change the project membership or UriPatterns.  They can also become invalid if you send sensor data with timestamps more than a day old.  In either of these cases, use the Projects page of the ProjectBrowser to clear the caches associated with the Project that has been changed._


---


## 1.0 From client-server to SOA ##

In Hackystat Versions 1 to 7, the system provided a standard two-tier client server architecture.  Here's a diagram illustrating that architecture with a representative set of capabilities:

![http://hackystat.googlecode.com/svn/wiki/caching.1.gif](http://hackystat.googlecode.com/svn/wiki/caching.1.gif)

This architecture was simple, and as long as the system remained simple, it
worked quite well.  Unfortunately, by Version 7, the system had become
quite complex, with many more capabilities than present in the diagram above, and we recognized that
in order to better support future development, we needed to decompose the "server" into
a set of services communicating via HTTP:

![http://hackystat.googlecode.com/svn/wiki/caching.2.gif](http://hackystat.googlecode.com/svn/wiki/caching.2.gif)

This architecture is in many ways a huge improvement.  First,  if you want to build a new Hackystat feature called "Foo" that depends only on, say, DailyProjectData, you can communicate with that service directly through HTTP:

![http://hackystat.googlecode.com/svn/wiki/caching.3.gif](http://hackystat.googlecode.com/svn/wiki/caching.3.gif)

You can write your Foo feature using virtually any language or framework you want, since it communicates with Hackystat through HTTP.  In previous versions of Hackystat, you needed to write Java and you needed to somehow integrate your new service directly into the web application.

Second, the new architecture is also more scalable.  Let's say that the web interface is getting bogged down.  Well, just bring up a second one on a different machine and tell some of your users to go there instead:

![http://hackystat.googlecode.com/svn/wiki/caching.4.gif](http://hackystat.googlecode.com/svn/wiki/caching.4.gif)

Many of the services can just as easily be replicated, leading to all sorts of different distributed architectures.

Third, you can more easily "drop in" new implementations of existing services.  Unlike the old architecture, where the divisions between components were difficult to understand, in the new architecture each service provides a well-defined API in terms of HTTP calls.  This makes it possible to experiment with improvements to one service with minimal impact to others.

## 2.0 The problem with SOA: from in-memory methods to HTTP networks ##

While the "service-oriented architecture" in Hackystat 8 has a number of strengths, it also creates one substantial new problem: what used to be accomplished with a single HTTP request in the old client-server architecture might now require dozens, hundreds, or even thousands of HTTP requests!

To see why this is so, let's consider an important Hackystat use case:  the Project Portfolio analysis:

![http://hackystat.googlecode.com/svn/wiki/caching.5.gif](http://hackystat.googlecode.com/svn/wiki/caching.5.gif)

In general, each histogram in the portfolio analysis corresponds to one telemetry chart providing trend data for a certain period of time. In the above display, the analysis is showing ten projects and ten trend lines, where each trend line corresponds to five weeks.

Let's look at how that translates into HTTP calls.   It begins by the user in their browser requesting the portfolio page from the web interface.  To build it, the web interface requests the trend data for the first analysis (Coverage) on the first project (hackystat-analysis-dailyprojectdata):

![http://hackystat.googlecode.com/svn/wiki/caching.6.gif](http://hackystat.googlecode.com/svn/wiki/caching.6.gif)

So far, not bad: just two HTTP requests.  Now, to build that trend data, the telemetry service needs to get some DailyProjectData instances.  In fact, it needs to get one DPD instance per day for the analysis interval.  I said that this analysis was for five weeks, so that's 35 days, or 35 HTTP request/response cycles to the DailyProjectData service for that first trend line:

![http://hackystat.googlecode.com/svn/wiki/caching.7.gif](http://hackystat.googlecode.com/svn/wiki/caching.7.gif)

Now we're up to a total of 37 HTTP requests.  This is already a terrible increase, but it gets even worse:  for each of these 35 DailyProjectData instances, the service might need to retrieve dozens to hundreds of sensor data instances from the SensorBase:

![http://hackystat.googlecode.com/svn/wiki/caching.8.gif](http://hackystat.googlecode.com/svn/wiki/caching.8.gif)

Let's assume each DPD instance requires 20 sensor data instances on average.  That means that for just one of the trend lines in the portfolio analysis, we have to get 1 (web interface) + 1 (Telemetry) + 35 (DailyProjectData) + 700 (SensorBase) = 737 HTTP requests.

Now we're up to 737 HTTP requests just to get our first trend line for our first project.
Unfortunately, for the entire portfolio analysis shown above, we have to get ten trend lines for each of ten projects, so we actually have 100 trend lines to retrieve.  That means that this single simple portfolio analysis could require 10 `*` 10 `*` 737 `=` 73,700 HTTP requests.  Yikes!

## 2.0 Caching to the rescue ##

Fortunately, one of the beautiful things about the [RESTful architecture](http://en.wikipedia.org/wiki/Representational_State_Transfer) is that it is well suited to caching.  In our case, the Telemetry service can cache the DailyProjectData instances it obtains from the DailyProjectData service.  Similarly, the DailyProjectData service can cache any SensorData instances it obtains from the SensorBase.

What this means is the following: the first time anyone in a project requests data, they have to pay the "full price" in terms of HTTP requests, but from that point on, everyone else can exploit the cached data to eliminate most of the computation and HTTP requests.

Analyses such as Portfolio are particularly well suited to this approach, since any given project member will typically run an analysis like Portfolio every few days over the past several weeks of data.  That means that most of the trend data will have been computed previously and be stored in one of the caches.  In the best case, all of it will have been previously cached, resulting in just 100 HTTP requests:

![http://hackystat.googlecode.com/svn/wiki/caching.9.gif](http://hackystat.googlecode.com/svn/wiki/caching.9.gif)

If each HTTP request takes about a half second, so that's about 50 seconds to display the above portfolio analysis, which is displaying trends in 10 measures for 10 projects for 35 days, or about 3,500 data points.

As you might expect, we could easily reduce the number of HTTP requests to 1 (for this best case scenario where all of the computation has been done previously) simply by adding a cache to the web interface service

![http://hackystat.googlecode.com/svn/wiki/caching.10.gif](http://hackystat.googlecode.com/svn/wiki/caching.10.gif)

Are our problems over?  Have we achieved the speed of a monolithic server along with the flexibility of SOA?  Unfortunately, not quite yet.

## 3.0 The problem with caching ##

To understand the problem with caching, it helps to think of it this way:  when you cache the results of a computation, such as a DailyProjectData instance or a Telemetry trend line, you are essentially taking a "snapshot" of the system at a single point in time and "freezing" it.  The benefit of the snapshot is that you can now save a tremendous amount of computation later.  The cost of the snapshot is the possibility that the state of the system could change in some way such that if you were to run the computation again, you would obtain a different result.  If that's the case, then your cached object has become "stale", or invalid.

There are many approaches to determining and/or minimizing the possibility of stale or invalid cached objects.  In the case of Hackystat, there are two kinds of objects that we currently cache: sensor data and DailyProjectData instances.

### 3.1 Cached sensor data: a non-problem ###

The first kind of object is sensor data.  Cached sensor data instances are extremely unlikely to become stale, because that happens only when an existing sensor data instance is overwritten with a new value.  In the seven years that we have been running Hackystat servers, overwriting sensor data instances has happened only a handful of times, and only when we as administrators were updating internal representations under controlled and monitored conditions.  We are not aware of any times that a user has intentionally overwritten their own sensor data instances with new values.

### 3.2 Cached DPD data: a big problem ###

The second kind of object is a DailyProjectData (DPD) instance.  As its name implies, a DPD instance summarizes the value of a certain kind of data about a project on a single day.  For example, one kind of DPD represents the number of times the project has been built on a single day.

There is one situation where it is obvious that a cached DPD instance might become invalid: that's when the DPD instance represents the current day.  Clearly, if you cache the number of times the project has been built on the current day, and then you go off and build the system again, then the cached value is now off by one.

The Hackystat caches for DPD instances are aware of this problem, and as a result never cache a DPD instance refering to the current day.  So, this situation cannot cause problems in practice.

There is a second situation for cached DPD instances that is far more problematic:  when the definition of the project itself changes. Recall that a project definition involves the following:
  * A set of members whose data should be included in project-level analyses.
  * A set of UriPatterns that specify which sensor data instances from each member should be included in project-level analyses.

So, let's say that Project Foo has members Joe and Sally, and the UriPattern "`*`/Foo/`*`", and that we've cached a Build DPD for 11/1/2008 that says the system has been built 10 times.

Now we change the definition of Project Foo to include a new member, Hoku.  The problem is that Hoku might have been building our project on 11/1/2008, or any other date associated with our project, and thus all of the cached instances could be invalid.

A similar situation can occur when changing the UriPattern: if we changed the UriPattern to be "`*`/foo/`*`", then a completely different number of builds could result.

There is a third situation in which a DPD instance can become invalid even when it's not the current day and when the project definition hasn't changed.  This is when a project member sends sensor data from the past to the server!  You might think this would require time travel, but it doesn't.  All it requires is a period during which you work in a manner disconnected from the Internet.  Let's say you go visit your relatives for the weekend and they don't have Internet access.  You continue to work on your code while you're there.  All this time, Hackystat sensors are storing your sensor data locally.  Once you return to civilization (and network access), your locally stored sensor data will be sent off to the server with the timestamps associated with the time of collection.  If another project member had been running analyses over the weekend, they could have cached DPD instances for Saturday or Sunday which are now invalid because upon your return on Monday morning, your computer sent off sensor data with timestamps for Saturday and Sunday.

### 3.3 When you need to worry about your caches ###

To summarize, there are two situations in which cached DPD data can become invalid:
  * When you change the membership or UriPatterns associated with a Project;
  * When you send data from more than a day in the past.  (It has to be over a day old, since Hackystat does not cache DPD instances from the current day).

Fortunately, both of these situations are infrequent in practice.

## 4.0 How to clear the DPD caches when you suspect they are bad ##

As of now, we do not have any automated mechanisms in place to detect when DPD caches need to be cleared.  Instead, we have simply implemented a way for users to clear their own caches.  On the Project page of the ProjectBrowser, there is a button associated with each project called "Clear Cache":

![http://hackystat.googlecode.com/svn/wiki/caching.11.gif](http://hackystat.googlecode.com/svn/wiki/caching.11.gif)

Pressing that button will send requests to the Telemetry and DailyProjectData services to clear all cached DPD instances associated with that project.

We agree that cache management should be more automatic, and the user should ideally not have to be aware of when the caches need to be cleared.  For the time being, however, it will remain something similar to the user experience with web browsers: sometimes you simply need to manually "refresh" the page or even clear the browser cache in order to get up to date results.
































