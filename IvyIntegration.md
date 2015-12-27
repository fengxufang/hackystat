**Table of Contents**


# Motivation #

The Hackystat system requires almost 20 third party libraries, as
summarized in the LibraryStandards page. Some libraries, such as
Checkstyle, are used by virtually every Hackystat system. Others, such as
Restlet, are used by only a few.  In addition, Hackystat itself is split up
into approximately 20 separate components, as summarized in the
ComponentDirectory page.  There are dependencies amongst these components
as well. For example, many components depend upon hackystat-utilities,
while none depend upon hackystat-sensor-emacs.

Currently, dependency management is done manually.  This means that users must manually:
  1. Download 15-20 libraries;
  1. Create and set an environment variable that provides the path to the library;
  1. Create and set a classpath variable for each environment variable (if using Eclipse);
  1. Ensure that they are using the correct version of each library.

This manual approach is resulting in the following problems:
  * One to two hours of painstaking configuration overhead to set up a new environment for Ant and Eclipse;
  * Difficulties understanding the dependencies associated with various projects.
  * Version problems, when the LibraryStandards page is out of date.
  * Version problems, when the version in the LibraryStandards page is no longer maintained.

There are two primary technologies for dependency management in Java:
[Maven](http://maven.apache.org/) (a project management tool that includes
dependency management) and [Ivy](http://ant.apache.org/ivy/) (a pure
dependency management tool).  I have looked at both of these technologies
in the past.  In the case of Maven, I feared that the overhead of requiring
Hackystat developers to learn this new system would outweigh its
advantages.  In the case of Ivy, the tool seemed promising but too
immature.

I now believe the time is right to move to Ivy, because: (a) as of two
months ago, Ivy had its first non-beta release (2.0.0), and now provides
reasonably good documentation and tutorials; (b) dependency management is
now a significant source of overhead, particularly for new developers;
(c) we have at least five new Hackystat developers starting up in a month
due to the Google Summer of Code program; and (d) a large number of our libraries are
out of date and require updating.

I've spent a day looking at Ivy, and the remainder of this document
summarizes my thoughts so far.  Please feel free to send your thoughts to the
hackystat-dev mailing list.


# Ivy in a nutshell #

The basic idea of Ivy can be understood in terms of: ivy.xml, ivysettings.xml, and local and remote repositories.

## ivy.xml ##

Each project contains a file called ivy.xml.  This file basically defines the project for Ivy and specifies what libraries this project depends upon.  For example:

```
<ivy-module version="2.0">
    <info organisation="org.apache" module="hello-ivy"/>
    <dependencies>
        <dependency org="commons-lang" name="commons-lang" rev="2.0"/>
        <dependency org="commons-cli" name="commons-cli" rev="1.0"/>
    </dependencies>
</ivy-module>
```

This ivy.xml defines a project (module) called "hello-ivy".  It also specifies that the hello-ivy project depends upon two libraries: commons-lang and commons-cli.

Note that the ivy.xml specifies _what_ dependencies (and what versions), but nothing about _where_ these dependencies should be found.  That information is provided by an entirely separate file, typically called ivysettings.xml.

## ivysettings.xml ##

The ivysettings.xml file provides a delarative way to specify how to search local or online repositories for libraries.  Here is an example ivysettings.xml file:

```
<ivysettings>
  <settings defaultResolver="chained"/>
  <resolvers>
    <chain name="chained" returnFirst="true">
      <filesystem name="libraries">
        <artifact pattern="${ivy.conf.dir}/repository/[artifact]-[revision].[type]" />
      </filesystem>
      <url name="integratebutton">
        <artifact pattern="http://www.integratebutton.com/repo/[organisation]/[module]/
           [revision]/[artifact]-[revision].[ext]" />
      </url>
      <ibiblio name="ibiblio" />
      <url name="ibiblio-mirror">
        <artifact pattern="http://mirrors.ibiblio.org/pub/mirrors/maven2/[organisation]/
           [module]/[branch]/[revision]/[branch]-[revision].[ext]" />
      </url>
    </chain>
  </resolvers>
</ivysettings>
```

This is a "chained" set of "resolvers", which means that it first tries the libraries filesystem for a given library specified in ivy.xml, then if it can't find it there, it will go on to try the integratebutton website, and so forth.

You can see that the ivysettings.xml file allows you to specify how the repository directory structure is laid out, as well as how the file names are structured.

## local and remote repositories ##

The ivysettings.xml file specifies how the system can query external repositories to look for a library of interest.  By default, it will consult the [Maven repository](http://mvnrepository.com/), but Ivy is designed to make it easy to support any kind of online repository.  For example, [Ivy Roundup](http://code.google.com/p/ivyroundup/) is an independent Ivy repository.

Ivy uses a local cache, typically located at ~/.ivy2/cache, to store downloaded files.  It then copies those files into your project directory, by default the lib/ directory.  This is very nice in the case of Hackystat where there are a large number of projects that depend upon the same set of libraries (Checkstyle, FindBugs, PMD, etc.)

# Ivy-Hackystat Integration Issues #

## Third party vs. Hackystat libraries ##

There are two very different kinds of dependencies to manage.  Third party libraries (checkstyle, etc.) change rather infrequently, while Hackystat libraries change quite frequently.  Furthermore, some Hackystat developers will prefer to use publically released binaries for libraries like hackystat-sensorbase-uh, while other developers will want to build this library from sources.

Ivy provides a "configuration" feature which enables control over things like whether you want a locally built version of a library vs. a publicly released one.

It might be the case that we migrate to Ivy in stages: first we support only third party libraries, then move to support for Hackystat libraries and configurations.

## Hackystat library repository ##

We will want Ivy to be able to download the binary distributions and unpack them.  Apparently the [Packager Resolver](http://ant.apache.org/ivy/history/2.0.0-rc1/resolver/packager.html) facility can allow this to happen, but I haven't looked at it yet.

## Eclipse integration ##

Ivy is an Ant-based mechanism, and so does not automate the configuration of Eclipse.  There are two approaches I hav found so far.  [!IvyDE](http://ant.apache.org/ivy/ivyde/) is an Eclipse plugin which read the ivy.xml file and add dependencies to your classpath.  [Ivy-Eclipse](http://code.google.com/p/ivy-eclipse/) is an Ant task that creates the Eclipse .classpath file based upon the ivy.xml file.  Another Ant task that does roughly the same thing is available in java source [here](http://www.digizenstudio.com/blog/2009/02/13/ivy-ant-task-to-maintain-eclipse-classpaths/).

## Where to put dependent libraries in projects? ##

Currently, our approach is for developers to download third party libraries to a single location that is published to Ant via an environment variable and published to Eclipse via a classpath variable.   Thus, individual project directories do not contain copies of third party libraries.

Under Ivy, this will probably change, as it wants to copy libraries out of its cache and into the local project directory structure.

The question is, where should these libraries go? By default, Ivy puts them in the project's lib/ directory, creating it if necessary.   Unfortunately, Hackystat projects typically have a lib/ directory with subdirectories for library-specific files (checkstyle, pmd, etc.), and this directory structure is under SVN control.

It is generally bad practice to have a directory hierarchy that mixes files that are under SVN control with files that are not.  One reason is because it can often lead to the files that are not under SVN control being accidentally committed to the repository.   More generally, the resulting directory structure is harder to understand.

We could tell Ivy to put these files somewhere under build/, such as in build/ivy or build/lib or build/jar.  This is good because it cleanly separates the files under SVN control from the files that aren't.  The problem is that if you run 'clean', you delete your third party libraries and have to regenerate them again.  This is not the end of the world, since those files are already locally cached, but it might confuse Eclipse.

I think the approach with the least amount of problems for us is to tell Ivy to put the files into a top-level project directory called "ivy".  This directory will not be under SVN control, and will not disappear when someone runs clean.  Let me know if you see problems with that or a better approach.

# Resources #

  * [Ivy](http://ant.apache.org/ivy/), currently at 2.1.0-rc1, which includes [Tutorials](http://ant.apache.org/ivy/history/2.1.0-rc1/tutorial.html), a [Reference Guide](http://ant.apache.org/ivy/history/2.1.0-rc1/reference.html), and an [FAQ](http://ant.apache.org/ivy/faq.html).

  * [IvyRoundup](http://code.google.com/p/ivyroundup/), an Ivy repository, and its [module directory](http://ivyroundup.googlecode.com/svn/trunk/repo/modules.xml).

  * [Automation for the people: manage dependencies with Ivy ](http://www.ibm.com/developerworks/java/library/j-ap05068/index.html), a nice introductory article on Ivy. (May, 2008)

  * [Ivy in 4.2 steps](http://www.testearly.com/2007/06/24/ivy-in-42-steps/), another intro article (from 2007, so dated)

  * [ApacheCon 2007 presentation on Ivy](http://ant.apache.org/ivy/presentations/apache-con-2007/)

  * [Pimpin your ant build with Ivy](http://blog.bdarfler.com/2008/11/11/pimpin-your-ant-build-with-ivy/), interesting post that talks about problems with FindBugs.

  * [Dependency management with Ant and Ivy](http://blogs.steeplesoft.com/dependency-management-with-ant-and-ivy/)

  * [Handling missing IvyRoundup files](http://vbossica.blogspot.com/2009/01/handling-missing-ivyoundup.html)

  * [My IvyRoundup Management Notes](http://docs.google.com/Doc?id=dd56f98r_25hjnjg6g7&hl=en)