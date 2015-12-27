**Table of Contents**


# 1.0 Introduction #

The Hackystat Framework is built from over a dozen independent Google Projects, as detailed in the [Component Directory](ComponentDirectory.md). Most of these projects have internal dependencies. For example, the Eclipse Sensor project depends upon the Sensor Shell project; the Sensor Shell project depends upon the SensorBase project, and the SensorBase Project depends upon the Utilities project.

In addition to internal dependencies, many Hackystat projects also have external dependencies to third-party libraries. For example, the [SensorBase](http://code.google.com/p/hackystat-sensorbase-uh/) project uses libraries from the [Restlet](http://www.restlet.org/) system.

The Hackystat system uses [Apache Ant](http://ant.apache.org/) and [Apache Ivy](http://ant.apache.org/ivy/) to support the downloading, installation, and maintenance of these internal and external dependencies.

The [Dependencies](Dependencies.md) page provides a listing of the current internal and external dependencies associated with the "released" Hackystat projects.

The remainder of this page documents the steps required to download and build the "released" Hackystat projects.

# 2.0 Downloading Hackystat Sources #

| Note: if you simply want to use Hackystat, you may find it easier to download a binary distribution rather than downloading sources and building locally.  See the [Installation Guide](InstallationGuide.md) for details. |
|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

Hackystat sources can be downloaded from Google Project Hosting using Subversion.  Because downloading over multiple projects individually can be irritating, we have developed an Ant script to simplify this process.  Here are the steps involved:

## 2.1 Install Java and Ant ##

Install [Java](http://java.sun.com/).  Hackystat requires Java 1.5 or above, but please use Java 1.6 or above.

Install [Ant](http://ant.apache.org/), and define the ANT\_HOME environment variable.  Hackystat requires Ant 1.7.1 or above.

## 2.2 Download the Hackystat top-level build.xml file ##

Create a top-level hackystat directory in which all of your hackystat sources will live.  This is typically called something like "hackystat-projects".  Due to Ant, please make sure the path to this directory does not contain spaces.  For example, "C:\Documents and Settings\hackystat-projects" would **not** be acceptable.

Download http://hackystat.googlecode.com/svn/trunk/build.xml into this directory. (Right-click this URL followed by "Save Link As.." should do the trick.)

This script defines a set of Ant targets with the following syntax: {CommandPrefix}.{Project}.  CommandPrefix is one of:  checkout, checkout.anonymous, commit, or status, and corresponds to the associated SVN command.  Project is the name of a released Hackystat project, such as hackystat-utilities, hackystat-sensorbase-uh, etc. Project can also be "all", which refers to all released projects.

These Ant targets incorporate the internal Hackystat project dependencies documented in the [Page](Dependencies.md).

So, for example:
  * 'ant checkout.anonymous.hackystat-utilities' will SVN checkout (or update) just the hackystat-utilities project with read-only access.
  * 'ant checkout.anonymous.hackystat-sensorbase-uh' will SVN checkout (or update) both the hackystat-sensorbase-uh project and its dependent project, hackystat-utilities with read-only access.
  * 'ant checkout.all' will SVN checkout (or update) all released hackystat projects with commit access.
  * 'ant checkout.anonymous.all' will SVN checkout (or update) all released hackystat projects without commit access.
  * 'ant status.hackystat-utilities' will provide information on added and modified files in the hackystat-utilities workspace.
  * 'ant -Dsvn.message="Fix typos" commit.all' will commit all changes to all Hackystat projects with the log message "Fix typos".

The checkout.anonymous.`*` and status.`*` commands do not require SVN credentials, but the checkout.`*` and commit.`*` commands do.  To provide these credentials, you must create a file called svn.properties containing these credentials (documented below).

The first time you invoke this script, you must invoke the "install" target, which will download various files necessary for SVN and Ivy.  If you forget to do this, the script will fail with an error message indicating you need to invoke 'ant install'.  Here is an example of the output from running that command:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ant install
Buildfile: build.xml

install:

install.library.versions.properties:
      [get] Getting: http://hackystat.googlecode.com/svn/trunk/configfiles/library.versions.properties
      [get] To: /Users/johnson/.hackystat/library.versions.properties
      [get] Not modified - so not downloaded

install.ivy:
      [get] Getting: http://repo1.maven.org/maven2/org/apache/ivy/ivy/2.1.0-rc1/ivy-2.1.0-rc1.jar
      [get] To: /Users/johnson/.ivy2/ivyjar/ivy.jar
      [get] Not modified - so not downloaded

install.ivysettings.xml:
      [get] Getting: http://hackystat.googlecode.com/svn/trunk/configfiles/ivysettings.xml
      [get] To: /Users/johnson/hackystat-projects-example/ivysettings.xml

install.svnkit:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/ivysettings.xml

BUILD SUCCESSFUL
Total time: 2 seconds
philip-johnsons-computer-2:hackystat-projects-example johnson$ 
```

Running this install target will download a file called library.versions.properties that provides the version numbers for third party libraries used by Hackystat. It also downloads the Ivy binary, as well as the Ivy file (ivysettings.xml) that indicates what repositories are accessed by Ivy for Hackystat dependent libraries. Finally, it downloads the SVNkit library and stores it in lib/svnkit.

Here is what the top-level directory looks like after running the install target:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ls
build.xml	ivysettings.xml	     lib/
```


## 2.3 Checkout sources to your local workspace ##

If you simply want to build the sources without committing any changes, you can checkout the source "anonymously" using one of the checkout.anonymous.`*` commands.

### 2.3.1 Checkout sources with read-only access ###

For example, here is the result of running the checkout.anonymous.hackystat-sensorbase-uh command. Note that checking out this project results in its dependent Hackystat project being checked out as well:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ant checkout.anonymous.hackystat-sensorbase-uh
Buildfile: build.xml

check-install:

checkout.anonymous.hackystat-utilities:
     [java] A    hackystat-utilities/sclc.build.xml
     [java] A    hackystat-utilities/.project
     [java] A    hackystat-utilities/junit.build.xml
     (deleted)
     [java]  U   hackystat-utilities
     [java] Checked out revision 217.

checkout.anonymous.hackystat-sensorbase-uh:
     [java] A    hackystat-sensorbase-uh/sample.sensorbase.properties
     [java] A    hackystat-sensorbase-uh/python
     [java] A    hackystat-sensorbase-uh/python/Sensorbase
     (deleted)
     [java]  U   hackystat-sensorbase-uh
     [java] Checked out revision 598.

BUILD SUCCESSFUL
Total time: 33 seconds
philip-johnsons-computer-2:hackystat-projects-example johnson$ 
```

If you invoke this command again, it has the effect of SVN update, so you can use this same command for initial checkout and to ensure your local workspace is consistent with the Google Project Hosting repository.

You cannot commit changes to projects that you have checked out anonymously. If you want to commit changes, you must delete these projects (or create a new directory) and checkout the projects with your SVN credentials, as discussed in the next section.


### 2.3.2 Checkout sources with read-write access ###

To checkout sources with read-write (commit) access, you must first create a file called "svn.properties" and place it in your hackystat-projects directory.  This file should look like this:

```
svn.username=johnsmith
svn.password=v8f4k2a9
```

Where "johnsmith" is your Google account and "v8f4k2a9" is the SVN password (not your Google password!).  Find your SVN password in your settings page: http://code.google.com/hosting/settings

For example, here is the top-level directory just before checking out a project with read-write access:
```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ls
build.xml	ivysettings.xml	lib		svn.properties
```

Now we invoke the "checkout.hackystat-utilities" command:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ant checkout.hackystat-utilities
Buildfile: build.xml

check-install:

check-svn-properties:

checkout.hackystat-utilities:
     [java] A    hackystat-utilities/sclc.build.xml
     [java] A    hackystat-utilities/.project
     [java] A    hackystat-utilities/junit.build.xml
     (deleted)
     [java]  U   hackystat-utilities
     [java] Checked out revision 217.

BUILD SUCCESSFUL
Total time: 9 seconds
philip-johnsons-computer-2:hackystat-projects-example johnson$ 
```

If you invoke this command again, it has the effect of SVN update, so you can use this same command for initial checkout and to ensure your local workspace is consistent with the Google Project Hosting repository.

## 2.4 Getting the status of your local workspace ##

It is often useful to know the status of your local workspace: whether you have made changes to files or added new ones.  The build script provides a set of  "status" commands to facilitate this.

Let's assume that you've been working on the sensorbase and want to know about any changes you have made to this (or any dependent) projects.  Here is an example of the status.hackystat-sensorbase-uh command:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ant status.hackystat-sensorbase-uh
Buildfile: build.xml

check-install:

status.hackystat-utilities:

status.hackystat-sensorbase-uh:
     [java] M       hackystat-sensorbase-uh/src/org/hackystat/sensorbase/client/SensorBaseClient.java

BUILD SUCCESSFUL
Total time: 1 second
```

This command reveals that the file SensorBaseClient.java has been modified.  It also checks the status of the dependent project (hackystat-utilities) which has not been changed.

## 2.5 Committing changes to your local workspace ##

To commit changes, invoke one of the commit.`*` commands.  As with all the other commands, it will do a commit of all dependent targets.  You must always supply the -Dsvn.message argument on the command line with a string that will be used as the commit log message.

Here is an example:

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ ant -Dsvn.message="Fix javadoc typos" commit.hackystat-sensorbase-uh 
Buildfile: build.xml

check-install:

check-svn-properties:

check-svn-message:

commit.hackystat-utilities:

commit.hackystat-sensorbase-uh:
     [java] Sending        hackystat-sensorbase-uh/src/org/hackystat/sensorbase/client/SensorBaseClient.java
     [java] Transmitting file data .
     [java] Committed revision 599.

BUILD SUCCESSFUL
Total time: 6 seconds
```


# 3.0 Building Hackystat Projects #

Once you have checked out the sources, whether anonymously or with commit access, the next step is to build them.  There are many ways to build the projects depending upon your goals as a developer.  In general, you will want to read the Developer Guide pages associated with a given project.

This section documents the simplest possible approach to building, which is the "standard" approach used for integration builds.  Every Hackystat Project must provide an Ant file called "verify.build.xml" which is called by the integration build scripts in order to compile and test the project. Every Hackystat project must also provide an Ant file called "dist.build.xml" which creates binary executables (such as .jar or .war files) and packages them for distribution.

## 3.1 Building an individual Hackystat Project ##

| Note: Before building a project, you may wish to invoke one of the checkout or checkout.anonymous commands documented above to ensure that you have the latest version of the sources. |
|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

The released Hackystat projects all tend to have a similar build system structure. In particular, the following commands are essential for building Hackystat projects.

| **Command** | **Description** |
|:------------|:----------------|
| ant         | The default build command, which invokes Ivy to obtain all internal and external dependencies, then compiles the current system. |
| ant -f jar.build.xml publish | Compiles the current system, builds the jar file, then invokes Ivy to "publish" the jar file to a local Ivy repository. The most recent "published" version of a Hackystat internal dependency is used by all dependent projects.|
| ant -f jar.build.xml publish-all | Invoke "publish" on all dependent projects in order, and finally compile and publish the current project. |

Let's illustrate how this works with some simple examples.

First, let's build the hackystat-utilities project by cd'ing to its directory and invoking "ant":

```
philip-johnsons-computer-2:hackystat-projects-example johnson$ cd hackystat-utilities
philip-johnsons-computer-2:hackystat-utilities johnson$ ant
Buildfile: build.xml
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/configfiles

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-utilities/ivysettings.xml

compile:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/classes
    [javac] Compiling 37 source files to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/classes

BUILD SUCCESSFUL
Total time: 2 seconds
```

The default target invokes compile, which depends upon install-libraries.  The install-libraries target installs any external dependencies. In the case of hackystat-utilities, it will install Apache JCS, util.concurrent, commons-logging, and junit into subdirectories of the lib directory:

```
philip-johnsons-computer-2:hackystat-utilities johnson$ ls lib/
commons-logging/	concurrent/	configfiles/	jcs/		junit/
```

After installing these dependencies, it invokes the compile command, which succeeds.

Now that we've built hackystat-utilities, it would seem to be correct to cd to the hackystat-sensorbase-uh directory and build it:

```
philip-johnsons-computer-2:hackystat-utilities johnson$ cd ../hackystat-sensorbase-uh/
philip-johnsons-computer-2:hackystat-sensorbase-uh johnson$ ant
Buildfile: build.xml

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/ivysettings.xml
[ivy:retrieve] 
[ivy:retrieve] :: problems summary ::
[ivy:retrieve] :::: WARNINGS
[ivy:retrieve] 		module not found: org.hackystat#hackystat-utilities;latest.integration
[ivy:retrieve] 	==== roundup: tried
[ivy:retrieve] 	  http://ivyroundup.googlecode.com/svn/trunk/repo/modules/org.hackystat/hackystat-utilities/[revision]/ivy.xml
[ivy:retrieve] 	  -- artifact org.hackystat#hackystat-utilities;latest.integration!hackystat-utilities.jar:
[ivy:retrieve] 	  http://ivyroundup.googlecode.com/svn/trunk/repo/modules/org.hackystat/hackystat-utilities/[revision]/packager.xml
[ivy:retrieve] 	==== local-repository: tried
[ivy:retrieve] 	  /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/[revision]/hackystat-utilities-[revision].xml
[ivy:retrieve] 	  -- artifact org.hackystat#hackystat-utilities;latest.integration!hackystat-utilities.jar:
[ivy:retrieve] 	  /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/[revision]/hackystat-utilities-[revision].jar
[ivy:retrieve] 		::::::::::::::::::::::::::::::::::::::::::::::
[ivy:retrieve] 		::          UNRESOLVED DEPENDENCIES         ::
[ivy:retrieve] 		::::::::::::::::::::::::::::::::::::::::::::::
[ivy:retrieve] 		:: org.hackystat#hackystat-utilities;latest.integration: not found
[ivy:retrieve] 		::::::::::::::::::::::::::::::::::::::::::::::
[ivy:retrieve] 
[ivy:retrieve] :: USE VERBOSE OR DEBUG MESSAGE LEVEL FOR MORE DETAILS

BUILD FAILED
/Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build.xml:62: impossible to resolve dependencies:
	resolve failed - see output for details

Total time: 1 second
```


Unfortunately, we get an error because we failed to "publish" the hackystat-utilities project.  That is easy to fix by invoking 'ant -f jar.build.xml publish' in the hackystat-utilities project:

```
philip-johnsons-computer-2:hackystat-sensorbase-uh johnson$ cd ../hackystat-utilities
philip-johnsons-computer-2:hackystat-utilities johnson$ ant -f jar.build.xml publish
Buildfile: jar.build.xml

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-utilities/ivysettings.xml

compile:

jar-validate-email:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar
     [copy] Copying 2 files to /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
      [jar] Building jar: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar/validate.email.lib.jar
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib

   (deleted)

jar-time:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
     [copy] Copying 23 files to /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
      [jar] Building jar: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar/time.lib.jar
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib

jar:
     [copy] Copying 7 files to /Users/johnson/hackystat-projects-example/hackystat-utilities

publish:
[ivy:resolve] :: resolving dependencies :: org.hackystat#hackystat-utilities;working@admin01.ics.hawaii.edu
[ivy:resolve] 	confs: [default]
[ivy:resolve] 	found org.junit#junit;4.5 in chained
[ivy:resolve] 	found edu.oswego.cs#concurrent;1.3.4 in chained
[ivy:resolve] 	found org.apache.commons#commons-logging;1.1.1 in chained
[ivy:resolve] 	found org.apache.jcs#jcs;1.3 in chained
[ivy:resolve] :: resolution report :: resolve 60ms :: artifacts dl 8ms
	---------------------------------------------------------------------
	|                  |            modules            ||   artifacts   |
	|       conf       | number| search|dwnlded|evicted|| number|dwnlded|
	---------------------------------------------------------------------
	|      default     |   4   |   0   |   0   |   0   ||   12  |   0   |
	---------------------------------------------------------------------
:: delivering :: org.hackystat#hackystat-utilities;working@admin01.ics.hawaii.edu :: 2009.07.06.12.37.57 :: integration :: Mon Jul 06 12:37:57 HST 2009
	delivering ivy file to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar/ivy.xml
:: publishing :: org.hackystat#hackystat-utilities
	published hackystatlogger.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/hackystatlogger.lib-2009.07.06.12.37.57.jar
	published hackystatuserhome.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/hackystatuserhome.lib-2009.07.06.12.37.57.jar
	published stacktrace.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/stacktrace.lib-2009.07.06.12.37.57.jar
	published time.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/time.lib-2009.07.06.12.37.57.jar
	published tstamp.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/tstamp.lib-2009.07.06.12.37.57.jar
	published uricache.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/uricache.lib-2009.07.06.12.37.57.jar
	published validate.email.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/validate.email.lib-2009.07.06.12.37.57.jar
	published ivy to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part/hackystat-utilities-2009.07.06.12.37.57.xml
	publish commited: moved /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57.part 
		to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57

BUILD SUCCESSFUL
Total time: 3 seconds
```

Now that the hackystat-utilities project has been published, we can build the hackystat-sensorbase-uh project:

```
philip-johnsons-computer-2:hackystat-utilities johnson$ cd ../hackystat-sensorbase-uh
philip-johnsons-computer-2:hackystat-sensorbase-uh johnson$ ant
Buildfile: build.xml

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/ivysettings.xml
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/hackystatlogger.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (6kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!hackystatlogger.lib.jar (5ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/time.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (31kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!time.lib.jar (12ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/hackystatuserhome.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (2kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!hackystatuserhome.lib.jar (6ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/uricache.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] ............ (672kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!uricache.lib.jar (48ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/validate.email.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (6kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!validate.email.lib.jar (4ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/stacktrace.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (2kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!stacktrace.lib.jar (3ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.37.57/tstamp.lib-2009.07.06.12.37.57.jar ...
[ivy:retrieve] .. (8kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.37.57!tstamp.lib.jar (3ms)

compile:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build/classes
    [javac] Compiling 106 source files to /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build/classes

BUILD SUCCESSFUL
Total time: 3 seconds
```

Let's look at the lib/ directory:

```
philip-johnsons-computer-2:hackystat-sensorbase-uh johnson$ ls lib/
configfiles/		derby/			hackystat-utilities/	junit/			restlet/
```

Ivy has now populated this directory with both the internal (hackystat-utilities) and external (derby, junit, and restlet) dependencies for the hackystat-sensorbase-uh project.

## 3.2 Building multiple projects ##

If you are making changes to multiple interdependent projects simultaneously, it can be tedious and error prone to manually rebuild and republish each project in the correct order.  Each Hackystat project has a "publish-all" target in the jar.build.xml file that forces all dependent projects to be published in order, thus guaranteeing that each project sees the latest version of each dependent project.

For example, when we failed to compile the hackystat-sensorbase-uh project above, we did not need to manually cd to the hackystat-utilities directory and publish it, then cd back to the hackystat-sensorbase-uh directory to invoke the build. Instead, we could have stayed in the hackystat-sensorbase-uh directory and invoked publish-all:

```
philip-johnsons-computer-2:hackystat-sensorbase-uh johnson$ ant -f jar.build.xml publish-all
Buildfile: jar.build.xml

publish-all:
Duplicated project name in import. Project build defined first in /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build.xml and again in /Users/johnson/hackystat-projects-example/hackystat-utilities/build.xml
Duplicated project name in import. Project ivy defined first in /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/ivy.build.xml and again in /Users/johnson/hackystat-projects-example/hackystat-utilities/ivy.build.xml
Duplicated project name in import. Project hackystat.sensors defined first in /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/hackystat.build.xml and again in /Users/johnson/hackystat-projects-example/hackystat-utilities/hackystat.build.xml
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-utilities/ivysettings.xml

compile:

jar-validate-email:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar
     [copy] Copying 2 files to /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
      [jar] Building jar: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar/validate.email.lib.jar
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-utilities/tmp-lib
   (deleted)

jar:
     [copy] Copying 7 files to /Users/johnson/hackystat-projects-example/hackystat-utilities

publish:
[ivy:resolve] :: resolving dependencies :: org.hackystat#hackystat-utilities;working@admin01.ics.hawaii.edu
[ivy:resolve] 	confs: [default]
[ivy:resolve] 	found org.junit#junit;4.5 in chained
[ivy:resolve] 	found edu.oswego.cs#concurrent;1.3.4 in chained
[ivy:resolve] 	found org.apache.commons#commons-logging;1.1.1 in chained
[ivy:resolve] 	found org.apache.jcs#jcs;1.3 in chained
[ivy:resolve] :: resolution report :: resolve 41ms :: artifacts dl 7ms
	---------------------------------------------------------------------
	|                  |            modules            ||   artifacts   |
	|       conf       | number| search|dwnlded|evicted|| number|dwnlded|
	---------------------------------------------------------------------
	|      default     |   4   |   0   |   0   |   0   ||   12  |   0   |
	---------------------------------------------------------------------
:: delivering :: org.hackystat#hackystat-utilities;working@admin01.ics.hawaii.edu :: 2009.07.06.12.57.30 :: integration :: Mon Jul 06 12:57:30 HST 2009
	delivering ivy file to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/jar/ivy.xml
:: publishing :: org.hackystat#hackystat-utilities
	published hackystatlogger.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/hackystatlogger.lib-2009.07.06.12.57.30.jar
	published hackystatuserhome.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/hackystatuserhome.lib-2009.07.06.12.57.30.jar
	published stacktrace.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/stacktrace.lib-2009.07.06.12.57.30.jar
	published time.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/time.lib-2009.07.06.12.57.30.jar
	published tstamp.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/tstamp.lib-2009.07.06.12.57.30.jar
	published uricache.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/uricache.lib-2009.07.06.12.57.30.jar
	published validate.email.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/validate.email.lib-2009.07.06.12.57.30.jar
	published ivy to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part/hackystat-utilities-2009.07.06.12.57.30.xml
	publish commited: moved /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30.part 
		to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30

publish-all:

install-libraries:
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/ivysettings.xml
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/stacktrace.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (2kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!stacktrace.lib.jar (4ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/validate.email.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (6kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!validate.email.lib.jar (4ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/uricache.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] ............ (672kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!uricache.lib.jar (34ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/hackystatlogger.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (6kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!hackystatlogger.lib.jar (3ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/tstamp.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (8kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!tstamp.lib.jar (3ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/hackystatuserhome.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (2kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!hackystatuserhome.lib.jar (4ms)
[ivy:retrieve] downloading /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-utilities/2009.07.06.12.57.30/time.lib-2009.07.06.12.57.30.jar ...
[ivy:retrieve] .. (31kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve] 	[SUCCESSFUL ] org.hackystat#hackystat-utilities;2009.07.06.12.57.30!time.lib.jar (6ms)

compile:

jar-standalone:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build/jar
     [copy] Copying 108 files to /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp
    [unjar] Expanding: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/lib/hackystat-utilities/hackystatlogger.lib.jar into /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp

    (deleted)

jar-lib:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp-lib
     [copy] Copying 108 files to /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp-lib
      [jar] Building jar: /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build/jar/sensorbase.lib.jar
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/tmp-lib

jar:
     [copy] Copying 3 files to /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh

publish:
[ivy:resolve] :: resolving dependencies :: org.hackystat#hackystat-sensorbase-uh;working@admin01.ics.hawaii.edu
[ivy:resolve] 	confs: [default]
[ivy:resolve] 	found org.junit#junit;4.5 in chained
[ivy:resolve] 	found org.restlet#restlet;1.1.5 in chained
[ivy:resolve] 	found org.apache.derby#derby;10.5.1.1 in chained
[ivy:resolve] 	found org.hackystat#hackystat-utilities;2009.07.06.12.57.30 in local-repository
[ivy:resolve] 	[2009.07.06.12.57.30] org.hackystat#hackystat-utilities;latest.integration
[ivy:resolve] 	found edu.oswego.cs#concurrent;1.3.4 in chained
[ivy:resolve] 	found org.apache.commons#commons-logging;1.1.1 in chained
[ivy:resolve] 	found org.apache.jcs#jcs;1.3 in chained
[ivy:resolve] :: resolution report :: resolve 610ms :: artifacts dl 75ms
	---------------------------------------------------------------------
	|                  |            modules            ||   artifacts   |
	|       conf       | number| search|dwnlded|evicted|| number|dwnlded|
	---------------------------------------------------------------------
	|      default     |   7   |   1   |   0   |   0   ||   99  |   0   |
	---------------------------------------------------------------------
:: delivering :: org.hackystat#hackystat-sensorbase-uh;working@admin01.ics.hawaii.edu :: 2009.07.06.12.57.45 :: integration :: Mon Jul 06 12:57:45 HST 2009
	delivering ivy file to /Users/johnson/hackystat-projects-example/hackystat-sensorbase-uh/build/jar/ivy.xml
:: publishing :: org.hackystat#hackystat-sensorbase-uh
	published sensorbase to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45.part/sensorbase-2009.07.06.12.57.45.jar
	published sensorbaseclient to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45.part/sensorbaseclient-2009.07.06.12.57.45.jar
	published sensorbase.lib to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45.part/sensorbase.lib-2009.07.06.12.57.45.jar
	published ivy to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45.part/hackystat-sensorbase-uh-2009.07.06.12.57.45.xml
	publish commited: moved /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45.part 
		to /Users/johnson/.ivy2/local-repository/org.hackystat/hackystat-sensorbase-uh/2009.07.06.12.57.45

BUILD SUCCESSFUL
Total time: 18 seconds
```

The publish-all command not only publishes the hackystat-utilities project, it also compiles, jars, and publishes the hackystat-sensorbase-uh project for use by any project that depends upon it.

# 4.0 Static analysis and verification #

## 4.1 Static analysis ##

Most Java-based Hackystat projects provide a set of build targets that perform various kinds of static analysis on the system. The following table describes the standard static analyses and how they are invoked:

| **Invocation** | **Description** |
|:---------------|:----------------|
| ant -f checkstyle.build.xml | Runs the checkstyle tool over the sources. See lib/configfiles for checkstyle configuration. |
| ant -f dependencyfinder.build.xml | Runs the dependencyfinder tool over sources, generating coupling information. |
| ant -f emma.build.xml | Runs the Emma code coverage tool to generate testing coverage data. |
| ant -f findbugs.build.xml | Runs the FindBugs tool to perform bytecode static analysis. See lib/configfiles for findbugs configuration. |
| ant -f junit.build.xml | Runs the JUnit tool, failing the build if any unit tests fail. |
| ant -f pmd.build.xml | Runs the PMD tool to perform source level static analysis. See lib/configfiles for PMD configuration. |
| ant -f sclc.build.xml | Runs the SCLC tool to generate source code size information.  Note that this system requires Active Python to be installed. |

The build system uses Ivy to automatically install the associated tool when you run the command for the first time.  For example, here is the result of invoking Checkstyle:

```
philip-johnsons-computer-2:hackystat-utilities johnson$ ant -f checkstyle.build.xml 
Buildfile: checkstyle.build.xml
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-utilities/ivysettings.xml

install-libraries:

compile:

checkstyle.install.config.file:
      [get] Getting: http://hackystat.googlecode.com/svn/trunk/configfiles//hackystat.checkstyle.xml
      [get] To: /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/configfiles/hackystat.checkstyle.xml

checkstyle.tool:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle
[checkstyle] Running Checkstyle 5.0 on 37 files

checkstyle.report:
     [xslt] Processing /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle/checkstyle.xml to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle/index.html
     [xslt] Loading stylesheet /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/checkstyle/checkstyle-noframes.xsl

checkstyle.sensor:
[hacky-checkstyle] 37 Checkstyle sensor data instances created.
[hacky-checkstyle] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (0 secs.)

checkstyle:

BUILD SUCCESSFUL
Total time: 6 seconds
```

This command downloaded the Checkstyle configuration file into lib/configfiles/hackystat.checkstyle.xml.  It also downloaded the Checkstyle binaries into lib/checkstyle.  Finally, it invoked the checkstyle tool.

## 4.2 Verification ##

Hackystat uses a Hudson server at http://dasha.ics.hawaii.edu:9870/ for continuous integration.  When you commit files associated with a released Hackystat project, the Hudson server will notice these commits within 5 minutes and invoke "ant -f verify.build.xml" to see if various static analysis tools can find problems.   If so, the build fails.

To avoid embarrassment, it is always best to invoke "ant -f verify.build.xml" prior to committing changes in order to ensure the code is clean.   For example:

```
philip-johnsons-computer-2:hackystat-utilities johnson$ ant -f verify.build.xml 
Buildfile: verify.build.xml
No ivy:settings found for the default reference 'ivy.instance'.  A default instance will be used
[ivy:retrieve] :: Ivy 2.1.0-rc1 - 20090319213629 :: http://ant.apache.org/ivy/ ::
:: loading settings :: file = /Users/johnson/hackystat-projects-example/hackystat-utilities/ivysettings.xml

clean:
   [delete] Deleting directory /Users/johnson/hackystat-projects-example/hackystat-utilities/build

install-libraries:

compile:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/classes
    [javac] Compiling 37 source files to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/classes

checkstyle.install.config.file:

checkstyle.tool:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle
[checkstyle] Running Checkstyle 5.0 on 37 files

checkstyle.report:
     [xslt] Processing /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle/checkstyle.xml to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/checkstyle/index.html
     [xslt] Loading stylesheet /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/checkstyle/checkstyle-noframes.xsl

checkstyle.sensor:
[hacky-checkstyle] 37 Checkstyle sensor data instances created.
[hacky-checkstyle] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (0 secs.)

checkstyle:

junit.tool:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/junit
    [junit] Running org.hackystat.utilities.email.TestValidateEmailSyntax
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.22 sec
    [junit] Running org.hackystat.utilities.home.TestHackystatUserHome
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.081 sec
    [junit] Running org.hackystat.utilities.logger.TestHackystatLogger
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.122 sec
    [junit] Error: 
    [junit] 07/06 14:00:09  (Test message2)
    [junit] 
    [junit] Running org.hackystat.utilities.stacktrace.TestStackTrace
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.08 sec
    [junit] Running org.hackystat.utilities.time.interval.TestDayInterval
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.196 sec
    [junit] Running org.hackystat.utilities.time.interval.TestIntervalUtility
    [junit] Tests run: 11, Failures: 0, Errors: 0, Time elapsed: 0.207 sec
    [junit] Running org.hackystat.utilities.time.interval.TestMonthInterval
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.106 sec
    [junit] Running org.hackystat.utilities.time.interval.TestWeekInterval
    [junit] Tests run: 2, Failures: 0, Errors: 0, Time elapsed: 0.2 sec
    [junit] Running org.hackystat.utilities.time.period.TestDay
    [junit] Tests run: 3, Failures: 0, Errors: 0, Time elapsed: 0.917 sec
    [junit] Running org.hackystat.utilities.time.period.TestMonth
    [junit] Tests run: 4, Failures: 0, Errors: 0, Time elapsed: 0.133 sec
    [junit] Running org.hackystat.utilities.time.period.TestWeek
    [junit] Tests run: 3, Failures: 0, Errors: 0, Time elapsed: 0.092 sec
    [junit] Running org.hackystat.utilities.tstamp.TestTstamp
    [junit] Tests run: 5, Failures: 0, Errors: 0, Time elapsed: 0.286 sec
    [junit] Running org.hackystat.utilities.tstamp.TestTstampSet
    [junit] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.079 sec
    [junit] Running org.hackystat.utilities.uricache.TestUriCache
    [junit] Tests run: 5, Failures: 0, Errors: 0, Time elapsed: 3.974 sec
    [junit] Shutting down TestSimpleCache cache.
    [junit] Shutting down GroupedKeyCache cache.
    [junit] Shutting down TestDisposeCache cache.
    [junit] Shutting down TestDiskCache cache.
    [junit] Shutting down TestExpiration cache.

junit.report:
[junitreport] Processing /Users/johnson/hackystat-projects-example/hackystat-utilities/build/junit/TESTS-TestSuites.xml to /var/folders/Ie/IexkyPe7HJ4viaAX-AXBYU+++TI/-Tmp-/null1277213914
[junitreport] Loading stylesheet jar:file:/Users/johnson/java/apache-ant-1.7.1/lib/ant-junit.jar!/org/apache/tools/ant/taskdefs/optional/junit/xsl/junit-frames.xsl
[junitreport] Transform time: 2670ms
[junitreport] Deleting: /var/folders/Ie/IexkyPe7HJ4viaAX-AXBYU+++TI/-Tmp-/null1277213914

junit.sensor:
[hacky-junit] 40 UnitTest sensor data instances created.
[hacky-junit] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (0 secs.)

junit:

pmd.install.rulesets.file:
      [get] Getting: http://hackystat.googlecode.com/svn/trunk/configfiles//hackystat.pmd.xml
      [get] To: /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/configfiles/hackystat.pmd.xml

pmd.tool:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/pmd
     [echo] PMD found 0 problem(s).

pmd.report:
     [xslt] Processing /Users/johnson/hackystat-projects-example/hackystat-utilities/build/pmd/pmd.xml to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/pmd/pmd-report-per-class.html
     [xslt] Loading stylesheet /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/pmd/pmd-report-per-class.xslt

pmd.sensor:
[hacky-pmd] 37 Code Issue sensor data instances created.
[hacky-pmd] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (0 secs.)

pmd:

findbugs.install.filter.file:
      [get] Getting: http://hackystat.googlecode.com/svn/trunk/configfiles//hackystat.findbugs.exclude.xml
      [get] To: /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/configfiles/hackystat.findbugs.exclude.xml

findbugs.tool:
    [mkdir] Created dir: /Users/johnson/hackystat-projects-example/hackystat-utilities/build/findbugs
 [findbugs] Executing findbugs from ant task
 [findbugs] Running FindBugs...
 [findbugs] The following classes needed for analysis were missing:
 [findbugs]   javax.mail.internet.InternetAddress
 [findbugs]   javax.mail.internet.AddressException
 [findbugs] Missing classes: 2
 [findbugs] Calculating exit code...
 [findbugs] Setting 'missing class' flag (2)
 [findbugs] Exit code set to: 2
 [findbugs] Java Result: 2
 [findbugs] Classes needed for analysis were missing
 [findbugs] Output saved to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/findbugs/findbugs.xml

findbugs.report:
     [xslt] Processing /Users/johnson/hackystat-projects-example/hackystat-utilities/build/findbugs/findbugs.xml to /Users/johnson/hackystat-projects-example/hackystat-utilities/build/findbugs/findbugs-default.html
     [xslt] Loading stylesheet /Users/johnson/hackystat-projects-example/hackystat-utilities/lib/findbugs/default.xsl

findbugs.sensor:
[hacky-findbugs] 37 CodeIssues sensor data instances created.
[hacky-findbugs] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (0 secs.)

findbugs:

verify:

BUILD SUCCESSFUL
Total time: 43 seconds
```

Note that running verify could take many minutes if the build system has not previously downloaded JUnit, Checkstyle, FindBugs, or PMD.

# 5.0 Miscellaneous Information #

## 5.1 You still need an SVN client ##

Although the top-level build.xml file provides facilities to checkout, commit, and assess the status of projects, it does not replace all the required SVN commands you will need to maintain a project. For example, it does not implement an SVN 'add' command, which is necessary if you are creating new files.   So, you will still need an SVN client for certain capabilities.

## 5.2 Cleaning the local repository ##

The build system publishes Hackystat projects to the ~/.ivy2/local-repository directory.  Each invocation of the "publish" target results in a new copy of the jar files associated with that project in the local-repository directory.

Eventually, quite a bit of space can be taken up by out of date versions of these files.  At present, we have not implemented any automated garbage collection on this directory; you'll have to occasionally clean it up yourself.   You can simply delete the entire directory and run "publish-all" as a simple form of management.

## 5.3 Screencast ##

A screencast covering much of this information is available at: http://code.google.com/p/hackystat/wiki/Screencasts#Building_Hackystat_From_Source.

## 5.4 Eclipse ##

Most Java-based Hackystat projects come with .project and .classpath files for use with Eclipse. Note that the .classpath files point to files in the project's lib/ directory for dependencies.  Thus, after checking out the sources, you must run Ant to build the system and populate the lib/ directory.  Only then can you open the Hackystat Eclipse project without build errors. Each time you "publish" dependent projects, use the Ecipse "refresh" command to load those changes into your Eclipse session.

## 5.5 From anonymous to commit access ##

One might initially check out the sources anonymously, then become a member and want to get the sources with read-write (commit) access.  Note that you cannot convert anonymously checked out sources to read-write "in place". Instead, you need to delete your read-only directories (or move them elsewhere), then check out a fresh version of the system, supplying the appropriate credentials.
