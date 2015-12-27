| **NOTE: As of Hackystat 8.3.713, JavaMail is installed using Ivy. This wiki page will remain temporarily as reference for prior versions.**|
|:-------------------------------------------------------------------------------------------------------------------------------------------|

**Table of Contents**



Some Hackystat Services, such as the [SensorBase](http://code.google.com/p/hackystat-sensorbase-uh/), send emails in response to certain conditions.  To support this behavior in Java, you should install JavaMail.  This page explains how to do this.

# 1. Download the JavaMail 1.4 libraries. #

The JavaMail libraries are available at: [http://java.sun.com/products/javamail/](http://java.sun.com/products/javamail/).

After downloading and unzipping, you should obtain a directory such as the following:

```
~/javamail-1.4/
```

It will contain a lib/ directory with the jar files that you will need to install.

## 1.1 (Java 5 only) Download the JavaBeans Activation Framework ##

If you are using Java 5, you must also download the JavaBeans Activation Framework 1.1 from [http://java.sun.com/products/javabeans/jaf/index.jsp](http://java.sun.com/products/javabeans/jaf/index.jsp).

If you are using Java 6, the activation framework comes pre-installed.

# 2. Copy JavaMail jar files to the {java}/lib/ext directory #

To install JavaMail, you need to copy mail.jar to the lib/ext directory.

If you are using Java 5, you also need to copy activation.jar from the JavaBeans Activation Framework library.

The location to which you copy these files depends upon your operating system.

| **OS** | **Typical location of lib/ext directory** |
|:-------|:------------------------------------------|
| Mac OS/X | /System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home/lib/ext  |
| Windows | C:\Java\jre1.6.0\_1\lib\ext, C:\Java\jdk1.6.0\_1\jre\lib\ext |

Note that in Windows, to be safe, you should install it in both the JRE and JDK lib/ext directories.

# 3. Verifying your installation #

Unfortunately, we don't currently have a simple way to verify that you have installed JavaMail correctly.  What we do know is that if you don't have JavaMail installed, then Hackystat services like the [SensorBase](http://code.google.com/p/hackystat-sensorbase-uh/) will print the following error message on startup:

```
05/24 11:57:29  ERROR: JavaMail not installed correctly! Mail services will fail!
```

So, one way to test your JavaMail installation is to run the SensorBase and see that this error message is not printed out.