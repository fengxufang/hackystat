This page guides you through the process of registering with a Hackystat SensorBase, downloading and installing sensors, and verifying that your sensors are sending data to the SensorBase.

Before going through this material, we suggest that you read the [Guided Tour](Tutorial_GuidedTour.md) so that you have a high level overview of Hackystat.

## 1.0 Register with a Hackystat SensorBase ##

In order to collect sensor data, you must have an account with a Hackystat SensorBase service.
You can download and install your own local SensorBase service and then register an account with it, or you can register an account with the public SensorBase server at http://dasha.ics.hawaii.edu:9876/sensorbase/register maintained by the [Collaborative Software Development Laboratory](http://csdl.ics.hawaii.edu) at the [University of Hawaii](http://www.hawaii.edu).  Whichever way you go, you will get the following screen:

![http://hackystat.googlecode.com/svn/wiki/register1.gif](http://hackystat.googlecode.com/svn/wiki/register1.gif)

After typing in your email address, the system will send you an email with the password to your newly created account:

![http://hackystat.googlecode.com/svn/wiki/register4.gif](http://hackystat.googlecode.com/svn/wiki/register4.gif)

## 2.0 Download the binary sensors distribution ##

Although it is possible to build the system from sources, and also possible to write your own sensors from scratch, the easiest way to get started is by using the latest binary distribution of sensors.  The latest binary distribution of Hackystat is available as the "Featured Download" on the [Hackystat Home Page](http://www.hackystat.org/):

![http://hackystat.googlecode.com/svn/wiki/featureddownload.gif](http://hackystat.googlecode.com/svn/wiki/featureddownload.gif)

Note that the Hackystat binary distribution is packaged as two downloadable files: a "sensors" binary distribution, which includes prebuilt sensors and other client-side packages, and a "services" binary distribution, which includes prebuilt server-side services such as the SensorBase.

You only need to download the "sensors" distribution in order to install a prebuilt sensor.

|Note: If the Hackystat home page does not specify Featured Downloads, click on the "Downloads" tab instead to access them. |
|:--------------------------------------------------------------------------------------------------------------------------|

The binary distribution expands into a top-level directory with the same name as the file (except for the .zip suffix), with a subdirectory for each sensor. For example:

```
c:\public_hackystat\hackystat-8.0.111-sensors-bin\
                              README.html
                              hackystat-sensor-ant\
                                                   antsensors.jar
                              hackystat-sensor-emacs\
                                                     sensor-package.el
                              hackystat-sensor-eclipse\
                                                       org.hackystat.sensor.eclipse_8.0.v20080111.jar
   :
   :
```

Note that the version number, 8.0.111, indicates the major release (8), the minor release (0), and the month/day of this build (111, or January 11).

Problems can arise if you unzip this package into a file path containing spaces (such as "c:\Documents and Settings\hackystat-8.0.111-sensors-bin").  Please be sure to always locate packages in directory paths that do not contain spaces, such as "c:\public\_hackystat\hackystat-8.0.111-sensors-bin".

## 3.0 Install and configure sensors ##

To install and configure a sensor, follow the instructions in its User Guide. Pointers to the User Guides for each sensor in the binary distribution can be found in the [Component Directory page](ComponentDirectory.md).

All sensors need access to the URL associated with your SensorBase, as well as your account and password.  In some cases, such as the Eclipse sensor, you will provide this information using an internal facility, such as the Eclipse preferences page.  In most cases, however, the sensor will access this information from the [sensorshell.properties file](http://code.google.com/p/hackystat-sensor-shell/wiki/SensorShellProperties).  The User Guide for the sensor will indicate what method is required.

## 4.0 Verify that your sensors are sending data ##

Once you have followed the directions for installing the sensor, you will want to verify that it is sending data to the SensorBase.  The easiest way to do that is with the ProjectBrowser.  If you are sending data to the public SensorBase, you can login to the public ProjectBrowser at http://dasha.ics.hawaii.edu:9879/projectbrowser/. Once logged in, you will be taken to the SensorData page, as illustrated here:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.1.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.1.gif)

Select the current month and year and the "Default" project, which is predefined to show all (and only) your own personal sensor data, and click submit.  The system will provide you with links to all of your sensor data for that month, as illustrated here:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.2.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.2.gif)

You can further drill down to see the individual sensor data instances by clicking on a link:

![http://hackystat.googlecode.com/svn/wiki/projectbrowser.3.gif](http://hackystat.googlecode.com/svn/wiki/projectbrowser.3.gif)

If you are not seeing sensor data from a tool, it is typically due to some sort of misconfiguration.  Some things to check:

  * Did you specify the Hackystat SensorBase host correctly?
  * Did you specify your Hackystat account correctly?
  * Did you type your Hackystat password correctly?

If all of those appear OK, please contact a Hackystat administrator for help.

## 5.0 Where to go from here? ##

Once you are successfully sending data to the SensorBase, the next step is to analyze it.  One way to analyze it is with Software Project Telemetry.  More details on Software Project Telemetry are available at:

  * [Software Project Telemetry Tutorial](Tutorial_SoftwareProjectTelemetry.md)

If you are working in a group, or are working on multiple tasks that you would like to analyze separately, then you will want to learn more about Hackystat Projects.  More details on Projects are available at:

  * [Hackystat Projects Tutorial](Tutorial_Projects.md)