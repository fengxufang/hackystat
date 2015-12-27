# A summary of Hackystat's history #

Work on Hackystat began on July 1, 2001. This first architectural release
family, called the "Spike Solution", was used to explore the feasibility of
the approach and to evaluate various component technologies (including
JATO, XSL, Ant, !JUnit, HttpUnit, Tomcat, Cocoon, and SOAP).  Work on this
Spike Solution architecture lasted eight months, from May to December,
2001.

In December 2001, we began a major re-implementation of the system in order
to provide architectural support for design discoveries made with the Spike
Solution. This second architectural release family is called the "Framework
Architecture", because it implements package and class-level patterns that
facilitate extension of the system via inheritance and composition of
existing classes. Unfortunately, the Framework architecture required
modification of the hackystat source code to implement new sensors and
analysis, and only a single configuration of analyses and sensors can be
supported in this architecture.

In November, 2002, we began work on the "SDK" (or "kernelized")
architecture release family.  This third architectural family decomposes
the system into two layers. The first layer is a kernel system which
implements the core facilities for data definition, storage, transmission
and user interface.  Developers build an actual Hackystat installation by
combining the framework with a second layer: a set of plug-in extension
modules that define the specific sensor data types, tool sensors, and
analyses required to support their development procedures.  In the SDK
architecture, developers can implement new sensors and analyses without
modifying the underlying kernel code, and multiple Hackystat installations
can be built with different configurations of analyses and sensors.

In June, 2003, we performed a package restructuring to facilitate the
development of a third layer, or "application" layer systems.  This is the
fourth architectural release family, called the "Three Layer" architecture.

After only a month of application layer development, we realized that the
build process we developed for the second release family would be woefully
inadequate to support the three layer architecture, in which multiple
components would be combined into a configuration for release. So, in
August, 2003, we released the fifth architectural release family, called
the "Component Architecture", along with extensive new build support
including the hackydev web site with daily integration builds.

In July, 2004, we released the sixth architectural family, which provides
services to support a "telemetry" based approach to software project
monitoring and control.  This includes various caching mechanisms, as well
as the hackyTelemetry package that includes a telemetry specification
language and associated APIs. In this architecture, telemetry support
becomes a "low level" feature of the system.

In December, 2005, we released the seventh architectural family, which
refactors the system into 63 modules organized into four component
subsystems. This architecture also includes a redesigned build system to
simplify external development and external integration of new modules by
third-party developers.  Unlike prior architectures, which were designed
for extension only at the "leaves" of the system, the new architecture
supports growth in any of the four component subsystems.

In May, 2007, we began working on the eight architectural family, which
implements a web service architecture using REST principles. Almost the
entire system was rewritten for this version. The public release of
this system occurred in January, 2008.

# Release History #

## Version 8: REST Architecture Release Family (May 2007 - present) ##

### 15-Jan-2008 8.0.115 ###

  * REST architecture
  * Hosted at Google Projects.
  * Derby/RDBMS back end.
  * Restlet framework.


## Version 7: Subsystem Architecture Release Family (December 2005 - May 2007) ##

### 01-May-2007 7.7.501 ###

  * New sensors for Clover, TestNG, JDepend
  * Visual Studio support for Zorro TDD inference.
  * Core and SDT subsystems upgraded to Java 5.
  * Quiescent modules moved out of build system.
  * Developer Guide chapter on sensor design

### 23-Dec-2006 7.6.1223 ###

  * Significant improvements to Zorro application for TDD inference
  * New sensor for Idea
  * Improvements to Visual Studio sensor
  * User interface validity checking improvements
  * New SOAP implementation based upon Apache Axis

### 18-Oct-2006 7.5.1018 ###
  * Update to require Tomcat 5.5.20
  * Update to require Java 1.5
  * Experimentation chapter for Administrator guide
  * New sensor for Bugzilla
  * New "DevTime" analyses
  * New DailyProjectData design
  * Significant improvements to Zorro TDD inference engine
  * Significant improvements to Telemetry engine

### 07-June-2006  7.4.608 ###
  * New Telemetry Language feature: Filter Functions.
  * New chapters on Telemetry, Test Driven Design, Administration.
  * PingMail: Administrator server monitoring program
  * Privacy policy page configurable
  * SDT structures evolved to new versions
  * New Sensor for Open Office


### 17-March-2006 7.3.120 ###

  * New SDT: DevEvent
  * New/Improved Sensors: SCLC, Jira, XmlData, FindBugs, Vim
  * Multi-language size counting/analysis
  * Substantial documentation improvements: new Developer Guide chapters, rewritten "StackyHack" chapter, new Reference Guide

### 20-January-2006 7.2.120 ###

  * A new modular, docbook-based documentation providing configuration-specific User, Administrator, and Developer guides.
  * Improvements to !hackyApp\_BuildAnalysis.
  * Improvements to hackyInstaller.

### 06-December-2005 7.0.1206 ###
  * Complete refactoring of system into 63 modules organized into four subsystems.
  * Complete redesign of build system, providing substantial performance, maintainability, and extensibility improvements.
  * Support for evolution in sensor data types.
  * New Sensor Data Type: "Code Issue"
  * New sensors for: PMD, Emma, FindBugs, Checkstyle, Subversion
  * Stable releases are now binary distributions (30 MB rather than 180 MB)
  * Improved property file design (hackystat.build.properties and hackystat.site.properties)
  * CCCC sensor now works with standard version of CCCC.
  * Hackystat now Java5 compliant (backward compatible to JDK 1.4.2)
  * Hackystat server now runs on Tomcat 5.5
  * Jira sensor works with SSL.
  * Hackystat source repository now hosted using Subversion.


## Version 6: Telemetry Release Family (July 2004-November 2005) ##

### 14-October-2005 6.8.1014 ###

  * Many improvements and bugfixes for HackyInstaller, including support for UserMaps, and Proxy settings.
  * Release of hackyExperiment package.
  * Eclipse 3.1 sensor support
  * CLI Bash sensor support

### 09-September-2005 6.7.909 ###

  * New sensor-side installation system called HackyInstaller

### 13-July-2005 6.6.713 ###

  * Revisions to telemetry definition language and charting to supply multiple axes
  * Rewrite of telemetry documentation chapter
  * Improvements to Priority Ranked Inspection module
  * Improvements to Continuous GQM module
  * Improvements to Software Review analysis module
  * New configurations: Hackystat-HPC and Hackystat-Standard

### 08-May-2005 6.5.508 ###
  * User Guide Chapter on Software Telemetry
  * Revisions to telemetry definition language
  * Improvements to Issue sensor
  * New configurations: Hackystat-HPC and Hackystat-Standard

### 23-Feb-2005 6.4.223 ###

  * New User Guide based upon DocBook
  * Redesigned Ant sensor and Build sensor data type
  * Redesigned Jira sensor and Issue sensor data type
  * Daily Diary and Alert to support "journaling"

### 10-Dec-2004 6.3.1210 ###

  * New sensor and SDT for "dependencies", supporting static coupling analysis.
  * New sensor and SDT for "issues", supporting issue management systems like Jira.
  * Analyses to support high performance computing size measures (MPI and C++)
  * Workspace mappings are persisted, improving performance significantly for long projects.

### 16-Oct-2004 6.2.1016 ###

  * Telemetry now supports 'templates' to reduce number of required defined streams and charts.
  * Project-level Daily Summary Alert and Details analyses now available. Guided Tour 03 updated with examples.
  * Jira issue database is now publically browsable, and provides "roadmap" to current and future releases.
  * Build and BCML data collection now optional in hackyEclipse.
  * Jupiter sensor for review data now available.

### 03-Sept-2004 6.1.903 ###

  * TelemetryViewer for Telemetry Control Center
  * http://www.hackystat.org is new URL for developer services
  * Guided Tour for new users (narrated PowerPoint).
  * Article: Improving Software Development Management through Software Project Telemetry
  * Mailing lists now archived at The Mail Archive
  * New issue management system using Jira.
  * Initial release of review sensor
  * Improvements for Unix, Vim, Telemetry

### 07-July-2004 6.0.707 ###

  * Initial release of Build and Perf sensor data types
  * Initial release of 'telemetry' language and associated processing
  * Improvements to LOCC size counting for C++ and sensor
  * Initial release of Hackystat load testing sensor.
  * Initial release of Vim sensor
  * Substantial thread-safety improvements
  * Substantial JavaDoc improvements
  * Improvements to Microsoft Office sensor.
  * Improvements to the CVS commit sensor.
  * Eclipse sensor for 3.0 major release
  * Improvements to standard extensions package classes
  * Elimination of cactus and servlet-runner based testing

## Version 5: Component Architecture Release Family (August 2003-June 2004) ##

### 28-April-2004 5.5.428 ###

  * Initial release of a Microsoft Office sensor.
  * Initial release of a CVS commit sensor.
  * Initial release of a command line invocation sensor.
  * Eclipse sensor updates for 3.0 M7 and M8 builds.
  * Project 'churn' analysis.
  * Use of interval selector.
  * Modifications to support improved data from JPL.

### 15-March-2004 5.4.315 ###

  * LOCC sensor
  * Standardized analysis time interval selection
  * Event Sequence analyses (hackyTDD module)
  * Analysis invocation logging and reports
  * Thread safety improvements using util.concurrent
  * Project-based caching
  * Performance analysis data

### 05-Dec-2003 5.2.1205 ###

  * Support for storage/retrieval of international characters.
  * BCML sensor improvements.
  * Sensor installation documentation improvements.
  * Project active time and file analyses.
  * Eclipse update notification.

### 25-Oct-2003 5.1.1025 ###

  * Analyses for Build and State Change analysis.
  * Reorganization of analyses and groups.


### 15-Oct-2003 5.1.1015 ###

  * Enhanced chart facilities, including Gantt and Box-and-Whiskers.
  * HackyCourse module, supporting classroom data analysis and comparison.
  * Administrator enhancements for page display and organization.

### 14-Aug-2003 5.0.815 ###

  * Incorporation of Java Development Dynamics analyses.
  * Many minor bug removals.

### 01-Aug-2003 5.0.801 ###

  * Complete redesign of the build process.
  * Integration of Soap services into hackystat (backwards incompatible)
  * Configurations as first class entities in system specification


## Version 4: Three Layer Architecture Release Family (June 2003-July 2003) ##


### 04-June-2003 4.0.604 ###

  * Package renaming to org.hackystat.kernel.


## Version 3: SDK Architecture Release Family (Jan 2003-May 2003) ##

### 5/02/2003 3.6.502 ###

  * Complexity command and alert for threshold monitoring
  * JBlanket configuration changes for coverage monitoring
  * WorkspaceMap mechanism for mapping Java classes to workspaces

### 4/09/2003 3.5.409 ###

  * Cache mechanism supports noGC option.

### 4/01/2003 3.4.401 ###

  * BuffTrans Daily Analysis
  * Improvements to the Eclipse sensor

### 3/14/2003 3.3.314 ###

  * Initial release of the Visual Studio sensor.
  * Initial release of Chart API
  * Several example chart-based analyses.
  * JBlanket sensor that sends Coverage SDT data.

### 2/28/2003 3.2.228 ###

  * Initial release of the CCCC sensor.
  * Simple daily statistics analysis
  * Coverage Sensor Data Type (no coverage sensor yet)

### 2/21/2003 3.1.221 ###

  * Initial release of an Eclipse sensor.
  * Daily analysis subsystem and Daily Diary analysis
  * Workspace subsystem and preferences (was Locale)
  * Analysis Road Map documentation page
  * User Interface Definition documentation page
  * BuffTrans SDT and Emacs sensor

### 1/24/2003 3.0.124 ###

  * Sensors: JBuilder, Emacs, and JUnit (via Ant)
  * Sensor Data Types:  Activity, UnitTest, and FileMetric
  * Analyses:  List Sensor Data, Day Data Summary
  * Preferences: Locale Window
  * Alert:  Daily Data Alert


## Version 2: Framework Architecture Release Family (Feb 2002-Dec 2002) ##

### 11/05/2002 2.1105 ###

  * New command: Locale Metrics for summarizing activity in a given module.
  * New command: Development Curve for summarizing "hotspots" in Java code development.
  * New command: Weekly Review.
  * Emacs sensor now clears the hackystat shell buffer when it gets large.
  * Performance analysis of sensor log code to improve memory and cpu utilization.

### 10/04/2002 2.1004 ###

  * Server-side Active/Idle time calculation changed for consistency with Weekly Review statistics. Now, the user is only "active" when a state changed event exists within a five minute interval.
  * Midnight roll-over bug in ActivityLog fixed. This means that people working past midnight, or uploading offline data will have their data correctly stored.
  * CkMetrics updated to calculate LOC directly through the .class file LineNumberTable data structure.
  * Fixed JUnitSensor bug in which different test cases could be potentially sent with the same time stamp.
  * New coder-decoder implemented for SOAP string transmission; should be faster and less error prone (no special "delimiter" characters required).

### 9/01/2002  2.0901 ###

  * JBuilder sensor updated so the OOSize data is computed over all open Java files in browser, not just currently active file.
  * JBuilder sensor updated to work under JBuilder7.
  * Installation instructions for JUnit sensor fixed so that sensorshell.jar is downloaded.


### 8/01/2002  2.0801 ###

  * SensorShell component implemented, providing services to all sensors including log files, temporary storage, autosend, offline storage, and improved efficiency. Documentation available in the doc/ directory.
  * Emacs sensor now includes OoSize computation.
  * Emacs, JUnit and JBuilder sensors upgraded to use SensorShell.
  * BCEL component recompiled with a fix to the Repository class. This allows updates to CkMetrics data during a single session.
  * CkMetrics upgrades to facilitate NOC computation and to reduce amount of .class file parsing.
  * Logging available on all sensors in uniform manner using SensorShell.
  * SendNotification eliminated.
  * sensor.properties file updated: new values HACKYSTAT\_STATE\_CHANGE\_INTERVAL and HACKYSTAT\_AUTOSEND\_INTERVAL.
  * Checkstyle upgraded to version 2.3.  1,000 new checkstyle errors fixed.
  * testOne ant target prompts the user for a test file name prefix, and executes only those test cases whose names start with the prefix.


### 7/10/2002 2.0710 ###

  * Object Oriented metrics data is now available.  The CKMetrics package collects the Chidamber-Kemerer metrics by parsing the .class file associated with a java source. The OOSize Sensor Log stores this information and a JBuilder-based sensor is available.
  * Most Active File is now computed from StateChange events.
  * Complexity Threshold Alert is available, which informs the developer when the CKMetrics of any of the recent Most Active Files exceeds user-specified thresholds.
  * Locale data is now available. Locales allow the developer and analysis functions to "localize" any computations to a "region" of the system. The "window" of days to be analyzed for locale information is user-configurable. Locales can be hidden from appearing in the pull-down menus. Analyses allow restriction to a locale.
  * The Daily Diary is updated to include Most Active Files, Locale Information, OOSize information, and JUnit data.
  * System has been tested on Unix (Solaris) and Windows. Jitender reports that it runs on Linux as well.
  * A new setEnv.sh script exists for Bash shell.
  * Improvements to online documentation.


### 5/21/2002 2.0521 ###

  * The JUnitSensor for Jakarta Ant is re-released.  The new version batches transmission of all test data into a single HTTP request, improving performance substantially.
  * JUnit data now appears in the Daily Diary page.


### 5/03/2002 2.0503 ###

  * A chart package based upon JFreeCharts is now available, and an example chart showing Active and Idle time is available in the home page.
  * Image caching is disabled (i.e. javax.imageio.ImageIO.setUseCache(false)). This is done because the default cache location is apparently not accessible in Tomcat 4.0.3.
  * Tested on Tomcat 4.0.3 and Tomcat 4.0.3-LE.
  * As a result of the chart implementation which uses the javax.imageio package, Hackystat no longer compiles under Java 1.3.  The JBuilder.html file is updated to explain how to upgrade your JBuilder6 system to use jdk1.4 for compilation. You will also have to add additional (chart related) jar files to the JBuilder lib.
  * Server upgrade instructions updated to explain how to upgrade Tomcat and port your Hackystat data over to the new Tomcat installation.

### 4/26/2002 2.0426 ###

  * Cocoon has been eliminated from the distribution.  The removal of Cocoon cuts the size of the distribution in half, and makes Hackystat more portable across versions of Tomcat and Java.
  * User home page is improved.
  * Many bug fixes to Daily Diary page.
  * Sensor Log data can be retrieved in either XML or HTML format.
  * Server upgrade instructions now available.
  * Extensive improvements to user guide.


### 4/11/2002 2.0411 ###

  * Provides a link back to the user's home page on (almost) all pages.
  * Provides an improved user configuration page using the new Configuration Manager implementation.
  * Cleans up the user home page so that form fields provide useful default values.
  * Provides an initial implementation of the Daily Diary page. To be fixed in next release.


### 2.0404 4/4/2002 ###


  * Implements the basic Alert processing infrastructure. Two alerts are currently implemented: one detects bad data received during the previous day (BadDataAlert) and one detects regular sensor data received during the previous day (DataAlert).  Both are enabled for users by default.

### 2.0313 3/13/2002 ###
  * Implements the following improvements to JBuilder and Emacs sensors. Users must re-download all sensor executable files (.el and .jar) from the sensor installation page to their local computers to obtain these improvements:
  * Implements the "State Change" activity type (both Emacs and JBuilder sensors). This activity type is implemented by a timer that wakes up once per minute and checks to see if the active buffer has changed or the size of the buffer has changed. If either is true, then a new State Change activity is posted.
  * Implements sending of activity time data after a certain period of idle time (both Emacs and JBuilder sensors). If the user appears to be idle for a certain period of time (15 or 30 minutes, it doesn't really matter) then the current set of activity events (if any) will be sent to the server.
  * Fixes bug in Emacs sensor that displayed only first digit of the number of activities being sent to the server in the minibuffer. (For example, if 12 activities were sent, it would say "Sending 1 activities.."


### 2.036 3/6/2002 ###


  * Provides DailyEmail services.  Each user will receive an email from the system the day following the receipt of data by the server. Currently, the email just informs the user that data was received and provides a link to their user home page. In future, the email will include any alerts triggered by analysis of the data.
  * Provides the ACTIVE\_BUFFER\_STATE\_CHANGE activity type, in preparation for timer-based activity sensors.


### 2.022 2/22/2002 ###

  * Component upgrades: Java (1.4), Tomcat (4.0.1), Soap (2.2), Cocoon (2.0.1), Ant (1.4.1), HttpUnit (1.3), JUnit (3.7)
  * Incorporation of JSP pages and Java Standard Tag Libraries (JSTL) as primary user interface technology.
  * Build.xml redesigned and now includes JUnitReport, LOCC, Checkstyle, and J2H tasks.
  * War file-based deployment.
  * JBuilder 6 compatibility.
  * Package-level structure redesigned to explicitly separate client, server, and common code.
  * Notification code refactored to include abstract base class (Send).
  * Sensor Log redesigned and refactored; all Sensor Logs are daily and use StartTime as their key.
  * Sensor Log Entries redesigned and refactored to ensure that they meet Effective Java criteria for TreeMap storage.
  * Users use their 12 character key rather than email to identify themselves to the server (either during Login or through notifications).
  * User home page redesigned and improved.
  * "Interestingness" is now a property independent of Sensor Logs.
  * Validity checking now implemented on notification data and integrated with Bad Data Log.
  * HackystatAdmin directory hierarchy eliminated and replaced with user.properties file.
  * Certain environment variables eliminated (HACKYSTAT\_HOME).
  * MVC "Model 2" pattern implemented for user interface code.
  * SensorLogInfo class provides an API to MVC controllers that simplify inspection of user state.
  * Session Log eliminated. The Spike Solution efforts discovered that it was redundant and that Activity Log data should be used to calculate session information.  This also niftily solves the "overlapping session data problem" that existed when a developer had two IDEs going simultaneously.
  * Idle time specification and calculation moved from the client to the server side.
  * Creation of a sendnotification.jar component that combines mail.jar, activation.jar, soap.jar, xerces.jar, and hackystat.jar and simplifies command-line notification invocation.
  * SendNotification is now passed the host and key explicitly and does not access environment variables.
  * Creation of a sensor-package.el file that combines the five component .el files for the Emacs Lisp sensor into a single file.
  * Emacs sensor download reduced from 10 files to 2.
  * Emacs sensor works on both GNU Emacs and XEmacs.
  * LicenseInfo.html file provided with complete license information on all Hackystat components.
  * JUnit sensor redesigned. Instead of hacking the JUnit source and producing a custom .jar file, the JUnit sensor now reads the XML files produced by the JUnit ant task and generates notifications based upon that data.
  * User guide updated and improved.
  * Style sheets used to factor out interface settings.
  * Monthly calendar page now shows a single month at a time with navigation arrows to go to next/previous month.
  * JUnit test cases extended and improved.
  * JavaPrintf package removed.
  * SaveFileInterval mechanism removed.  Sensor Logs are now written to disk after every update.
  * Binary/Source distribution targets consolidated.  Only one (source) distribution made.
  * CSDL Tools/Hackystat page now provides online access to JavaDocs and sources of latest distribution.
  * Sensor configuration now stored in a dot directory (".hackystat").

## Version 1: Spike Solution Architecture Release Family (Jul 2001-Dec 2001) ##

### 12/18/2001 ###

  * Provides bad data logging

### 10/25/2001 ###

  * Provides calendar-based interface
  * JBuilder sensor indicates when saving files.
  * News email includes JUnit test data.

### 9/28/2001 ###

  * Provides idle time analysis.
  * Provides defect data (JUnit) collection.
  * Provides unified sensor configuration through sensor.properties
  * Provides JBuilder4 and JBuilder5 compatibility
  * Improved daily summary file

### 8/28/2001 ###

  * Provides timer-based emails to users with a summary of session information.
  * File saves are timer-based.
  * Ping notification type implemented.


### 7/26/2001 ###

  * ActivityLogs now kept in daily files.
  * Configuration page provided.

### 7/12/2001 ###

  * Initial "public" server release with two sensors for ToolTime data collection.