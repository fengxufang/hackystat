## 1.0 Introduction ##

[JAXB](https://jaxb.dev.java.net/) is a Java technology for bi-directional translation between XML and Java.  JAXB has two components:

  1. A compile-time component, called the JAXB compiler, or XJC.  This system reads in an XML schema and generates .java files containing Java class definitions corresponding to the definitions in the XML schema.
  1. A run-time library.  Among other things, the JAXB run-time library provides implementations of a "Marshaller" class (for translating Java instances to their corresponding XML) and an "Unmarshaller" class (for translating XML to a set of Java class instances).

JAXB is very convenient for dealing with XML, but there are several infrastructure issues that must be addressed in Hackystat:

  * Java 5 did not contain JAXB at all, while Java 6 provides JAXB 2.0 as a core library. This implies that Java 5 users must install JAXB into their Java run-time environment.
  * JAXB now has a new major release, called 2.1.5, which is not completely backward compatible with JAXB 2.0.

A requirement for Hackystat is to run successfully in both Java 5 and Java 6 environments, and to support both JAXB 2.0 and JAXB 2.1.5. The implications of this requirement are:

  * If you are running Java 5, you must install JAXB (2.0 or (preferably) 2.1.5) into your Java runtime environment.
  * If you are building Hackystat and need to run the XJC compiler because you have added or altered XML schemas, you must use the JAXB 2.1.5 XJC compiler.
  * Hackystat requires the JAXB 2.1.5 XJC compiler, but the Hackystat JAXB Ant tasks are configured by default to generate JAXB 2.0-compliant files. This allows Hackystat binaries to execute in a runtime with either JAXB 2.0 or JAXB 2.1.5.
  * By setting an environment variable, the default Hackystat JAXB compilation can be overridden, allowing the task to  generate 2.1-compliant Java class files.  This enables a developer to build a Hackystat executable with full optimizations for a JAXB 2.1.5-compliant runtime, but which will not run in a JAXB 2.0 runtime (such as the default Java 6 environment).

The next section discusses how to install JAXB into a Java 5 environment, which is all that end-users of Hackystat should need to worry about.  The following sections discuss issues of interest to developers only.

## 2.0 How to install JAXB into Java 5 ##

### 2.1 Downloading JAXB ###

The JAXB libraries are available at: [JAXB](https://jaxb.dev.java.net/).

You can install either JAXB 2.0 or JAXB 2.1.5.

After downloading and invoking 'java -jar JAXB-{version}.jar', you should obtain a directory such as the following:

```
~/jaxb-ri-20070122/  # for JAXB 2.0
```

or

```
~/jaxb-ri-20070917/  # for JAXB 2.1.5
```

It will contain a lib/ directory with the jar files that you will need to install.

### 2.2 Copy JAXB jar files to the {java}/lib/ext directory ###

To install JAXB into the Java 5 runtime, you need to copy the following four files from the JAXB/lib directory:

  * activation.jar (**see below**)
  * jaxb-api.jar
  * jaxb-impl.jar
  * jsr173\_1.0\_api.jar

| **Note:** you have presumably already installed version 1.1 of the JavaBeans Activation Framework as part of InstallingJavaMail, which is more recent than than the 1.0 version provided with JAXB 2.0, so you do not need to copy over the activation.jar file. However JAXB 2.1.X includes JavaBeans Activation Framework 1.1 so in that case it doesn't really matter whether you replace the activation.jar or not. |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

The location to which you copy these files depends upon your operating system.

| **OS** | **Typical location of lib/ext directory** |
|:-------|:------------------------------------------|
| Mac OS/X | /System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home/lib/ext  |
| Windows | C:\Java\jre1.5.0\_7\lib\ext               |

Note that in Windows, you must be careful to install it in the "JRE" directory, not the "JDK" directory. On Mac OS X you will need to authenticate as an administrator to copy the files to the System area, and these libraries may be overwritten if you upgrade the operating system in the future.

### 2.3 Verifying your installation ###

Unfortunately, we don't currently have a simple way to verify that you have installed JAXB correctly.  What we do know is that if you don't have JAXB installed, then Hackystat services like the [SensorBase](http://code.google.com/p/hackystat-sensorbase-uh/) will abort with the following error on startup:

```
admin01:~/svn-google/sensorbase-uh johnson$ java -jar sensorbase.jar 
/Users/johnson/.hackystat/sensorbase/sensorbase.properties not found. Using default sensorbase properties.
08/09 10:17:57  Derby: previously initialized.
Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/JAXBContext
        at org.hackystat.sensorbase.resource.sensordatatypes.SdtManager.<init>(SdtManager.java:87)
        at org.hackystat.sensorbase.server.Server.newInstance(Server.java:94)
        at org.hackystat.sensorbase.server.Server.main(Server.java:130)
```

Sensors might get errors such as the following:

```
java.lang.LinkageError: loader constraints violated when linking javax/xml/names pace/QName class

java.lang.NoClassDefFoundError
       at org.hackystat.sensorshell.SingleSensorShell.<init>(SingleSensorShell.java:132)
```

## 3.0 Hackystat Developers Guide to JAXB ##

### 3.1 Using JAXB with XJC ###

All Hackystat developers, just like end-users, must have an installed JAXB
runtime.  If you are not needing to run the XJC compiler, then if you are
running Java 6, you do not actually need to download JAXB, because the
pre-installed 2.0 runtime should be fine.

If you do need to run the XJC compiler, because you are editing and/or
adding XML schemas, then you need to download JAXB 2.1.5 and create an
environment variable called JAXB\_HOME that points to that directory.

Once you have done that, you can run the XJC compiler over the XML schemas using the jaxb.build.xml file.

Note that if you get an error related to the "target" attribute in the Ant task, this is due to your JAXB\_HOME
environment variable pointing to a JAXB 2.0 installation, not JAXB 2.1.5.

### 3.2 Overriding the default target JAXB version ###

All of the jaxb.build.xml files in the Hackystat projects share the following code structure:

```
<property environment="env" />
<!-- If the JAXB_TARGET env var is not defined, provide a default definition. -->
<property name="env.JAXB_TARGET" value="2.0"/>

  : 

<xjc target="${env.JAXB_TARGET}" 
     schema="${basedir}/xml/schema/DevTimeDailyProjectData.xsd" 
     destdir="src" 
     package="org.hackystat.dailyprojectdata.resource.devtime.jaxb">
  <produces dir="${basedir}/src/org/hackystat/dailyprojectdata/resource/devtime/jaxb" includes="*" />
</xjc>
```

What is important to notice here is that the Ant task reads in all environment variables, and if there is not an environment variable named JAXB\_TARGET, then it defines the associated property with the value "2.0".

The value of env.JAXB\_TARGET is then referenced in the XJC compiler task as the value of the target property.

Thus, by default, if you do not define an environment variable, the jaxb.build.xml file will produce 2.0 compliant Java class files.  This is the best general approach, as it means that the executables will work in standard Java 6 environments, for example.

If you want to take advantage of 2.1.3-specific properties, and you can guarantee that your system will always be run in an environment with a JAXB 2.1.3 runtime, then you can do the following:

  * Define the JAXB\_TARGET environment variable and set it to "2.1"
  * Run the XJC compiler for all Hackystat sensors and services of interest, using 'ant -f jaxb.build.xml'.  This will regenerate the Java class files in a 2.1.3-specific way.
  * Rebuild the sensors and services in the normal way, using, for example, "ant -f dist.build.xml".

Note that the public releases of Hackystat are not built in this way in order to avoid requiring JAXB updates.
