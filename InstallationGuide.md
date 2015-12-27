**Table of contents**


# 1.0 Introduction #

This page provides a guide to installing Hackystat services from a "Binary Distribution".

Note that most users do not need to install Hackystat services; instead, they will install Hackystat sensors and send their data to the public Hackystat services.  For information on how to install Hackystat sensors, see the [Sensor Data Collection Tutorial](http://code.google.com/p/hackystat/wiki/Tutorial_SensorDataCollection).

If you're still reading, that means you are interested in running Hackystat services, such as the SensorBase, in your local environment.

# 2.0 Downloading and configuration #

## 2.1 Install Java 6 ##

Although not a requirement of Hackystat, all of the current public Hackystat services are written in Java.  Thus, you should install [Java 6](http://java.sun.com/javase/6/).  (It is possible, but more error-prone, to use Java 5, and we will soon discontinue support for Java 5, so do yourself a favor and install Java 6.)

## 2.2 Downloading the latest binary distribution ##

We provide a link to the latest binary distribution of all "released" Hackystat services on the home page under "Featured Downloads":

![http://hackystat.googlecode.com/svn/wiki/installation.1.gif](http://hackystat.googlecode.com/svn/wiki/installation.1.gif)

You can see that there are two files listed as "Featured Downloads": one for sensors, and one for services.  You will want to download the "services" zip file.

|Note: If the home page does not provide any "Featured Downloads", click on the "Downloads" tab instead to obtain links to the downloads. |
|:----------------------------------------------------------------------------------------------------------------------------------------|

We recommend that you create a top-level directory called "public\_hackystat" and store all binary downloads in it.  For example, this screen image shows the results of downloading these featured releases and unzipping them:

![http://hackystat.googlecode.com/svn/wiki/installation.2.gif](http://hackystat.googlecode.com/svn/wiki/installation.2.gif)

It is a good idea to not download files into a directory path containing spaces. Thus, avoid directories like "Program Files".

The next screen image shows the basic structure of the services binary distribution.  The binary distribution consists of a number of subdirectories, each containing a service.  For example, this binary distribution contains the following services: hackystat-analysis-dailyprojectdata, hackystat-analysis-telemetry, hackystat-sensorbase-postgres, hackystat-sensorbase-uh, hackystat-ui-systemstatus, and hackystat-ui-wicket.

![http://hackystat.googlecode.com/svn/wiki/installation.2.1.gif](http://hackystat.googlecode.com/svn/wiki/installation.2.1.gif)

Inside each subdirectory are files (and possibly directories) supporting that service.  In the screen image above, you see the contents of the hackystat-sensorbase-uh service folder.  It contains a number of jar files, some "startup" batch files, a properties file, and so forth.  We will explain these in more detail below.

## 2.3 Define HACKYSTAT\_VERSION and HACKYSTAT\_SERVICE\_DIST environment variables ##

It is convenient to define two environment variables when installing Hackystat services, since these will simplify upgrading your system to future releases of the services.

Define HACKYSTAT\_VERSION as the current version you have downloaded.  On Windows, you define an environment variable by right-clicking on My Computer, then selecting Properties, then selecting the "Advanced" tab, then clicking the "Environment Variables" button, then clicking "New".   Here is an example:

![http://hackystat.googlecode.com/svn/wiki/installation.3.gif](http://hackystat.googlecode.com/svn/wiki/installation.3.gif)

Define HACKYSTAT\_SERVICE\_DIST to point to the directory containing the installation, and define that path using the value of HACKYSTAT\_VERSION.  Here is an example:

![http://hackystat.googlecode.com/svn/wiki/installation.4.gif](http://hackystat.googlecode.com/svn/wiki/installation.4.gif)

On Windows, you enclose an environment variable with '%'.

On a Macintosh, you achieve the same effect by editing the .profile file and using the export command as follows:
```
export HACKYSTAT_VERSION=8.1.1002
export HACKYSTAT_SERVICE_DIST=/Users/johnson/public_hackystat/hackystat-$HACKYSTAT_VERSION-services-bin
```

Once you have defined these variables, check your work by bringing up a new console window and changing directory to HACKYSTAT\_SERVICE\_DIST.  Here what it looks like in Windows:

![http://hackystat.googlecode.com/svn/wiki/installation.4.1.gif](http://hackystat.googlecode.com/svn/wiki/installation.4.1.gif)

If you don't get to the right directory, then fix your environment variables and try again.

## 2.4 Create and populate the ~/.hackystat/ directory ##

All Hackystat services refer to the .hackystat subdirectory in the user's home directory for local configuration information.  There are four "standard" services that you will typically set up: the sensorbase, dailyprojectdata, telemetry, and projectbrowser.

To start, create a directory called ".hackystat" in your home directory, and create four subdirectories in that directory called sensorbase, dailyprojectdata, telemetry, and projectbrowser.  The following screen image shows you what this looks like:

![http://hackystat.googlecode.com/svn/wiki/installation.5.gif](http://hackystat.googlecode.com/svn/wiki/installation.5.gif)

In the left hand pane above, you can see the contents of the .hackystat directory. In this case, there is a fifth subdirectory called "sensorshell", which is present since I have sensors installed on this computer as well.

On the right hand side, you can see that the sensorbase/ directory contains a file named "sensorbase.properties".  In addition, the dailyprojectdata/ directory contains a file called dailyprojectdata.properties, the telemetry/ directory contains telemetry.properties, and projectbrowser/ contains projectbrowser.properties.

Each service contains a file called "sample.{service}.properties", which you can use as a template.  The next step is to copy these sample files to the appropriate .hackystat subdirectory, and remove "sample." from their name. In other words:

| **Copy this file** | **to here and rename it** |
|:-------------------|:--------------------------|
|HACKYSTAT\_SERVICE\_DIST/hackystat-sensorbase-uh/sample.sensorbase.properties | ~/.hackystat/sensorbase/sensorbase.properties |
| HACKYSTAT\_SERVICE\_DIST/hackystat-analysis-dailyprojectdata/sample.dailyprojectdata.properties | ~/.hackystat/dailyprojectdata/dailyprojectdata.properties |
| HACKYSTAT\_SERVICE\_DIST/hackystat-analysis-telemetry/sample.telemetry.properties | ~/.hackystat/telemetry/telemetry.properties |
| HACKYSTAT\_SERVICE\_DIST/hackystat-ui-wicket/sample.projectbrowser.properties | ~/.hackystat/projectbrowser/projectbrowser.properties |

You must check each of these property files and change any properties as required to suit your local installation.  The properties files are documented here:
  * [sensorbase.properties](http://code.google.com/p/hackystat-sensorbase-uh/wiki/SensorBaseProperties)
  * [dailyprojectdata.properties](http://code.google.com/p/hackystat-analysis-dailyprojectdata/wiki/DailyProjectDataProperties)
  * [telemetry.properties](http://code.google.com/p/hackystat-analysis-telemetry/wiki/TelemetryProperties)
  * [projectbrowser.properties](http://code.google.com/p/hackystat-ui-wicket/wiki/ProjectBrowserProperties)

# 3.0 Bringing up services #

Each of the services comes with a pair of startup scripts to simplify bringing up the associated service.  They are named startup.{service}.bat (for Windows) and startup.{service}.command (for Macintosh).  These scripts are extremely simple. Here, for example, is the startup.sensorbase.bat script:
```
title = Sensorbase %HACKYSTAT_VERSION%
java -Xmx512M -jar %HACKYSTAT_SERVICE_DIST%\hackystat-sensorbase-uh\sensorbase.jar
```

All service scripts are essentially the same: they set the title of the associated console window, and then invoke the jar file associated with the specified service while increasing the maximum heap size to 512M.

It is nice (though not essential) to bring up the services in the following order:
  * Sensorbase
  * DailyProjectData
  * Telemetry
  * ProjectBrowser

This is because later services require connections to each of the earlier services, and test this connection on startup.  By bringing them up in order, you can ensure that you have configured things correctly.  (That said, it is perfectly reasonable to bring down any service temporarily and bring it back up without having to restart the other ones.)

Here is what you should see when bringing up the Sensorbase service:

![http://hackystat.googlecode.com/svn/wiki/installation.6.gif](http://hackystat.googlecode.com/svn/wiki/installation.6.gif)

Note that no errors or warnings are printed, and that it ends with "SensorBase (Version 8.1.1002 now running)".

Once you see that the sensorbase is running, you can bring up DailyProjectData by invoking its startup script, which produces a new console window:

![http://hackystat.googlecode.com/svn/wiki/installation.7.gif](http://hackystat.googlecode.com/svn/wiki/installation.7.gif)

Note that the service indicates that it contacted the Sensorbase successfully, and that it is now running.

Next, Telemetry:

![http://hackystat.googlecode.com/svn/wiki/installation.8.gif](http://hackystat.googlecode.com/svn/wiki/installation.8.gif)

The telemetry startup process is a little more complicated, but you can see near the bottom that it successfully contacted both the Sensorbase and the DailyProjectData services.

Finally, ProjectBrowser:

![http://hackystat.googlecode.com/svn/wiki/installation.9.gif](http://hackystat.googlecode.com/svn/wiki/installation.9.gif)

To check this service, bring up the URL noted at the bottom of the window (in this case, http://localhost:9879/projectbrowser):

![http://hackystat.googlecode.com/svn/wiki/installation.10.gif](http://hackystat.googlecode.com/svn/wiki/installation.10.gif)

Note that the bottom of the screen indicates that the Sensorbase, DailyProjectData, and Telemetry services were all successfully contacted.

# 4.0 Advanced configuration #

If you have been able to replicate the above, you will have successfully installed the four basic Hackystat services.  Congratulations!  Note that each service can have its own specialized configuration options:

  * The sensorbase can be configured to rely on a different backend database, such as Postgres, instead of the default (Derby).
  * DailyProjectData and Telemetry can be configured to use caching.
  * Telemetry can support prefetching and user defined charts.
  * ProjectBrowser can support optional pages and Portfolio configuration.

See the documentation for the individual services for more details on these capabilities.


# 5.0 Upgrading #

Once you have successfully installed Hackystat, upgrading to a new binary release is extremely simple:

  * Download the new service binary distribution into your public\_hackystat directory, and unzip it.
  * Edit your HACKYSTAT\_VERSION environment variable to the new version value.
  * Bring down currently running services (type control-c in their windows).
  * Reinvoke your scripts to bring up the new versions of the services.