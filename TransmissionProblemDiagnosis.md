## 1.0 Introduction ##

Sometimes sensor data is not transmitted efficiently or effectively from sensors to the sensorbase.  The goal of this page is to provide strategies for diagnosing problems if they occur and resolving them.

At a high level, transmission problems have only three possible causes:
  * The SensorBase server process is not responsive.
  * The sensor client process is not responsive.
  * The network is not responsive.

Some of the most likely reasons for a SensorBase server process to be unresponsive include:
  * The server process does not have adequate heap space.
  * The server is under too heavy a load.
  * The database implementation for the server is configured or implemented incorrectly.

Some of the most likely reasons for a sensor client process to be unresponsive include:
  * The client process does not have adequate heap space.
  * The client is configured or implemented incorrectly.

Some of the most likely reasons for a network to be unresponsive include:
  * Firewall issues.

The goal of this document is to provide some strategies that enable you as a Hackystat installer to isolate where problems are occurring and what you might want to do about them.

## 2.0 Diagnosis strategies ##

### 2.1 The minimalist configuration: Setup and invocation ###

If you are experiencing sensor data transmission problems, it is helpful to begin your diagnosis process by seeing if a minimal sensor data transmission scenario also displays the problems. If you can get a minimalist configuration to work reliably, it is much easier to proceed with the diagnosis.

Our recommendation for a minimalist sensor data transmission scenario is as follows:


First, use the default embedded Derby database implementation.  This eliminates code specific to your particular databaseimplementation.  The default embedded Derby database implementation is relatively high performance.  Be sure that:
  * You are allocating at least 256M of memory for heap.  The Java default of 64M is a known cause of sensor data transmission problems.
  * The logging level is set to INFO.  Setting the logging level to ALL or FINE is a known cause of transmission problems under heavy loads.

Second, use [ShellPerfEval](http://code.google.com/p/hackystat-sensor-shell/wiki/ShellPerfEval) as the "sensor".  This eliminates code specific to a particular sensor implementation.  Provide the following settings in the sensorshell.properties file read in by ShellPerfEval:
```
sensorshell.multishell.enabled = true
sensorshell.timeout = 200
sensorshell.timeout.ping = 200
sensorshell.multishell.numshells = 1
sensorshell.multishell.batchsize = 31
sensorshell.multishell.maxbuffer = 32
```

|Remember that ShellPerfEval uses the sensorshell.properties file in its own directory, not in ~/.hackystat/sensorshell. |
|:-----------------------------------------------------------------------------------------------------------------------|

The effect of these settings is to "throttle" the client-side transmission at a relatively low level: only one thread will be sending requests, each request will contain only 32 sensor data instances, and the client will wait up to 200 seconds before timing out.

You should also provide the client with at least 256M of heap space.

To test this scenario, bring up your test sensorbase as follows:
```
C:\hackystat-projects\hackystat-sensorbase-uh>java -Xmx256M -jar sensorbase.jar
Loading SensorBase properties from: C:\Documents and Settings\johnson/.hackystat/sensorbase/sensorbase.properties
03/12 14:30:30  Derby: previously initialized.
03/12 14:30:30  Loading SDT defaults from C:\hackystat-projects\hackystat-sensorbase-uh\xml\defaults\sensordatatypes.defaults.xml
03/12 14:30:30  Loading User defaults from C:\hackystat-projects\hackystat-sensorbase-uh\xml\defaults\users.defaults.xml
03/12 14:30:31  Loading Project defaults from C:\hackystat-projects\hackystat-sensorbase-uh\xml\defaults\projects.defaults.xml
03/12 14:30:33  Loading SensorData defaults: C:\hackystat-projects\hackystat-sensorbase-uh\xml\defaults\sensordata.defaults.xml
03/12 14:30:33  Host: http://localhost:9876/sensorbase/
03/12 14:30:33  SensorBase Properties:
                sensorbase.hostname = localhost
                sensorbase.context.root = sensorbase
                sensorbase.port = 9876
                sensorbase.admin.email = johnson@hawaii.edu
                sensorbase.admin.password = admin@hackystat.org
                sensorbase.db.dir = C:\Documents and Settings\johnson/.hackystat/sensorbase/db
                sensorbase.xml.dir = C:\hackystat-projects\hackystat-sensorbase-uh/xml
                sensorbase.db.impl = org.hackystat.sensorbase.db.derby.DerbyImplementation
                sensorbase.logging.level = INFO
                sensorbase.restlet.logging = false
                sensorbase.smtp.host = mail.hawaii.edu
                sensorbase.test.install = false
03/12 14:30:33  Maximum Java heap size (bytes): 266403840
03/12 14:30:33  SensorBase (Version 8.x.311) now running.
```

Note that the server prints out the maximum Java heap size it has available. You can use this to check that you have specified the heap size correctly.

Now, in a separate console window, run your ShellPerfEval.  The following run shows transmission of only 2000 instances, but you will typically want to send a higher number, such as 50,000 or 100,000.

```
C:\hackystat-projects\hackystat-sensor-shell>java -Xmx256M -jar shellperfeval.jar 2000
SensorProperties
  sensorshell.autosend.maxbuffer : 250
  sensorshell.autosend.timeinterval : 1.0
  sensorshell.logging.level : INFO
  sensorshell.multishell.autosend.timeinterval : 0.05
  sensorshell.multishell.batchsize : 31
  sensorshell.multishell.enabled : true
  sensorshell.multishell.maxbuffer : 32
  sensorshell.multishell.numshells : 1
  sensorshell.offline.cache.enabled : false
  sensorshell.offline.recovery.enabled : false
  sensorshell.sensorbase.host : http://localhost:9876/sensorbase/
  sensorshell.sensorbase.password : <password hidden>
  sensorshell.sensorbase.user : ShellPerfEval@hackystat.org
  sensorshell.statechange.interval : 30
  sensorshell.timeout : 200
  sensorshell.timeout.ping : 200
  sensorshell.properties file location: C:\hackystat-projects\hackystat-sensor-shell\sensorshell.properties
Number of milliseconds for each 'add' command, 50 per line:
15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 0 563 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 15 0 0 0 0 0 610 0 0 0 0 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 547 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 515 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 16 0 0 0 0 0 484 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 344 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 265 0 0 0 0 0 0 0 16 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 500 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 312 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 516 0 0 0 0 0 0 0 0 16 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 296 0 0 0 0 0 16 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 234 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 329 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 15 0 0 0 0 469 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 281 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 516 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
15 0 0 0 0 0 0 0 0 0 547 0 16 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 406 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 313 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 15 0
0 0 0 0 0 0 0 0 0 375 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 359 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 282 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 422 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 297 0 16 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 265 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 266 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 266 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 375 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 281 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 296 0 0 79 0 0 0 0 0 0 0
Finished: 1000 out of 2000
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 15 0 0 0 0 0 0 0 0 266 0 0 0 0 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 235 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 281 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 297 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 266 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 218 0 16 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 312 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 438 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 219 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 297 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 281 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 328 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 15 0 0 0 0 0 0 0 0 282 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 265 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 0 0 0 0 0 0 0 0 0 0 0 219 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 250 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 282 0 0 0 15 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 235 0 0 0 0 15 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 188 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 235 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 250 16 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 312 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0
219 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 219 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 15 0 0 0 0 0 0 0 0 0 0 281 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 453
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 219 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 406 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 0 0 0 0 0 281 0
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 16 0 0 0 0 250 0 0 0 0 0 0 0 0 15 0 0 0 0 0 0 0 0 0
Total time: 20765 milliseconds.
Milliseconds/sensordata instance: 10.3825
Deleting data from sensorbase... done.
```

This output shows a normal, correctly functioning mimimalist configuration.  There are 31 zeros in a row, which represent the queuing of sensor data instances locally which thus takes 0 milliseconds to complete, followed by a number between about 200 and 500, representing the number of milliseconds required to send these 32 instances from the client to the server.
Besides this output, it is also useful to look at the log file in ~/.hackystat/sensorshell/logs.  When multi-shell mode is enabled, the SensorShell will generate a separate log file for each interior shell for each run.  It does this by generating a timestamp for each run and appending a number to uniquely identify each of the shells.

In the case above, we specified only 1 shell, so only one file will be generated, which in this case was called "ShellPerfEval-1205368657335--0.0.log".  If 2 shells had been specified, then the other shell's logging output would have been in a file called "ShellPerfEval-1205368657335--1.0.log".

Here is the beginning and ending contents of the file called ShellPerfEval-1205368657335--0.0.log:
```
03/12 14:46:49  

03/12 14:46:49  Hackystat SensorShell Version: 8.x.312
03/12 14:46:49  SensorShell started at: Wed Mar 12 14:46:48 GMT-10:00 2008 
03/12 14:46:49  SensorProperties
  sensorshell.autosend.maxbuffer : 32
  sensorshell.autosend.timeinterval : 0.05
  sensorshell.logging.level : INFO
  sensorshell.multishell.autosend.timeinterval : 0.05
  sensorshell.multishell.batchsize : 31
  sensorshell.multishell.enabled : true
  sensorshell.multishell.maxbuffer : 32
  sensorshell.multishell.numshells : 1
  sensorshell.offline.cache.enabled : false
  sensorshell.offline.recovery.enabled : false
  sensorshell.sensorbase.host : http://localhost:9876/sensorbase/
  sensorshell.sensorbase.password : <password hidden>
  sensorshell.sensorbase.user : ShellPerfEval@hackystat.org
  sensorshell.statechange.interval : 30
  sensorshell.timeout : 200
  sensorshell.timeout.ping : 200
  sensorshell.properties file location: C:\hackystat-projects\hackystat-sensor-shell\sensorshell.properties
03/12 14:46:49  Type 'help' for a list of commands.
03/12 14:46:49  Host: http://localhost:9876/sensorbase/ is available.
03/12 14:46:49  User ShellPerfEval@hackystat.org is authorized to login at this host.
03/12 14:46:49  Maximum Java heap size (bytes): 266403840
03/12 14:46:49  AutoSend time interval set to 3 seconds
03/12 14:46:49  Pinged http://localhost:9876/sensorbase/ in 125 ms. Result is: true
03/12 14:46:49  Not checking for offline data: Offline recovery disabled.
03/12 14:46:49  Pinged http://localhost:9876/sensorbase/ in 15 ms. Result is: true
03/12 14:46:49  Invoking send(); buffer size > 32
03/12 14:46:49  #> send
03/12 14:46:49  Pinged http://localhost:9876/sensorbase/ in 15 ms. Result is: true
03/12 14:46:49  Attempting to send 33 sensor data instances. Remaining heap size (bytes): 261226496
03/12 14:46:50  Successful send to http://localhost:9876/sensorbase/
03/12 14:46:50  Invoking send(); buffer size > 32
03/12 14:46:50  #> send
03/12 14:46:50  Pinged http://localhost:9876/sensorbase/ in 16 ms. Result is: true
03/12 14:46:50  Attempting to send 33 sensor data instances. Remaining heap size (bytes): 261226496
03/12 14:46:50  Successful send to http://localhost:9876/sensorbase/
  :
  :
03/12 14:46:50  Invoking send(); buffer size > 32
03/12 14:47:10  #> send
03/12 14:47:10  Pinged http://localhost:9876/sensorbase/ in 31 ms. Result is: true
03/12 14:47:10  Attempting to send 33 sensor data instances. Remaining heap size (bytes): 261226496
03/12 14:47:11  Successful send to http://localhost:9876/sensorbase/
03/12 14:47:11  Pinged http://localhost:9876/sensorbase/ in 47 ms. Result is: true
03/12 14:47:11  #> quit
03/12 14:47:11  #> send
03/12 14:47:11  Pinged http://localhost:9876/sensorbase/ in 16 ms. Result is: true
03/12 14:47:11  Attempting to send 19 sensor data instances. Remaining heap size (bytes): 261226496
03/12 14:47:11  Successful send to http://localhost:9876/sensorbase/
03/12 14:47:11  Quitting SensorShell started at: Wed Mar 12 14:46:48 GMT-10:00 2008
03/12 14:47:11  Total sensor data instances sent: 2000
```

Note that this log file also specifies what heap size the client had available (at 14:37:37), which enables you to double check that the heap size has been increased correctly.  It also prints out the remaining heap size as transmission proceeds, which in this case does not change at all.

### 2.2 The minimalist configuration: If it works ###

If you've run your minimalist configuration in your actual network and it does not experience any sensor data transmission problems, the next step is to see if you can increase the transmission capacity on the client side. You can do this in several ways:
  * Increase the multishell.batchsize to values like 63, 127, or 255, and increase the multishell.maxbuffer proportionally, to 64, 128, or 256.  The maxbuffer should always be one more than batchsize.
  * Increase multishell.numshells to 2, 4, 6, etc.

### 2.3 The minimalist configuration: If it doesn't work ###

If the minimalist configuration doesn't work, then the next step is to figure out what is breaking. There are two common possibilities: server problems or network problems.

If the server cannot handle even this small of a load, it will be evidenced by the ShellPerfEval process taking longer and longer to send the 32 instances, until the timeout value of 200 seconds is reached, and the client process dies.   This should be reproducible; after restarting the server and client, the same behavior should occur each time.

If the network is unreliable, then it will be evidenced by the client process failing in the middle of a send.  Unlike the previous scenario, this should not be reproducible consistently, and the client process should fail at different points each time.

### 2.4 Trying a real sensor ###

Once you have explored the situation using ShellPerfEval, the next step is to exercise your actual sensors.

First, edit your ~/.hackystat/sensorshell/sensorshell.properties file to correspond to the settings you found appropriate from your experiments with ShellPerfEval.

Next, invoke the sensor with the property settings.  For example, here's an invocation of Ant with multishell enabled but just 1 shell. My build file uses the property hackystat.verbose.mode to set the verbose attribute of the sensor.

```
C:\hackystat-projects\hackystat-sensor-ant>ant -Dhackystat.verbose.mode=true -f pmd.build.xml
Buildfile: pmd.build.xml

pmd.tool:
      [pmd] No problems found!

pmd.report:
     [xslt] Processing C:\hackystat-projects\hackystat-sensor-ant\build\pmd\pmd.xml to C:\hackystat-projects\hackystat-sensor-ant\build\pmd\pmd-report-per-class.html
     [xslt] Loading stylesheet c:\java-tools\pmd-4.0\etc\xslt\pmd-report-per-class.xslt

pmd.sensor:
[hacky-pmd] verbose is set to: true
[hacky-pmd] Creating a SensorShell using sensorshell.properties data...
[hacky-pmd] SensorProperties
[hacky-pmd]   sensorshell.autosend.maxbuffer : 128
[hacky-pmd]   sensorshell.autosend.timeinterval : 0.05
[hacky-pmd]   sensorshell.logging.level : INFO
[hacky-pmd]   sensorshell.multishell.autosend.timeinterval : 0.05
[hacky-pmd]   sensorshell.multishell.batchsize : 127
[hacky-pmd]   sensorshell.multishell.enabled : true
[hacky-pmd]   sensorshell.multishell.maxbuffer : 128
[hacky-pmd]   sensorshell.multishell.numshells : 1
[hacky-pmd]   sensorshell.offline.cache.enabled : false
[hacky-pmd]   sensorshell.offline.recovery.enabled : false
[hacky-pmd]   sensorshell.sensorbase.host : http://dasha.ics.hawaii.edu:9876/sensorbase/
[hacky-pmd]   sensorshell.sensorbase.password : <password hidden>
[hacky-pmd]   sensorshell.sensorbase.user : johnson@hawaii.edu
[hacky-pmd]   sensorshell.statechange.interval : 30
[hacky-pmd]   sensorshell.timeout : 10
[hacky-pmd]   sensorshell.timeout.ping : 2
[hacky-pmd]   sensorshell.properties file location: C:\Documents and Settings\johnson\.hackystat\sensorshell\sensorshell.properties
[hacky-pmd] Maximum Java heap size is: 266403840
[hacky-pmd] Processing file: C:\hackystat-projects\hackystat-sensor-ant\build\pmd\pmd.xml
[hacky-pmd] Processing information about files that had PMD issues.
[hacky-pmd] Generating data for files that did not have PMD issues.
[hacky-pmd] 29 Code Issue sensor data instances created.
[hacky-pmd] These instances were transmitted to: http://dasha.ics.hawaii.edu:9876/sensorbase/ (1 secs.)

pmd:

BUILD SUCCESSFUL
Total time: 5 seconds
Sending build result to Hackystat server...
```

Since we have multi-shell enabled, each run of a sensor (such as the PMD sensor) will produce its own individual timestamped log file for each of the internal shell processes.  In this case, we find a single log file for PMD, which looks like this:

```
03/12 16:02:27  

03/12 16:02:27  Hackystat SensorShell Version: 8.x.312
03/12 16:02:27  SensorShell started at: Wed Mar 12 16:02:26 GMT-10:00 2008
03/12 16:02:27  SensorProperties
  sensorshell.autosend.maxbuffer : 128
  sensorshell.autosend.timeinterval : 0.05
  sensorshell.logging.level : INFO
  sensorshell.multishell.autosend.timeinterval : 0.05
  sensorshell.multishell.batchsize : 127
  sensorshell.multishell.enabled : true
  sensorshell.multishell.maxbuffer : 128
  sensorshell.multishell.numshells : 1
  sensorshell.offline.cache.enabled : false
  sensorshell.offline.recovery.enabled : false
  sensorshell.sensorbase.host : http://dasha.ics.hawaii.edu:9876/sensorbase/
  sensorshell.sensorbase.password : <password hidden>
  sensorshell.sensorbase.user : johnson@hawaii.edu
  sensorshell.statechange.interval : 30
  sensorshell.timeout : 10
  sensorshell.timeout.ping : 2
  sensorshell.properties file location: C:\Documents and Settings\johnson\.hackystat\sensorshell\sensorshell.properties
03/12 16:02:27  Type 'help' for a list of commands.
03/12 16:02:27  Host: http://dasha.ics.hawaii.edu:9876/sensorbase/ is available.
03/12 16:02:27  User johnson@hawaii.edu is authorized to login at this host.
03/12 16:02:27  Maximum Java heap size (bytes): 266403840
03/12 16:02:27  AutoSend time interval set to 3 seconds
03/12 16:02:27  Pinged http://dasha.ics.hawaii.edu:9876/sensorbase/ in 16 ms. Result is: true
03/12 16:02:27  Not checking for offline data: Offline recovery disabled.
03/12 16:02:28  #> quit
03/12 16:02:28  #> send
03/12 16:02:28  Pinged http://dasha.ics.hawaii.edu:9876/sensorbase/ in 31 ms. Result is: true
03/12 16:02:28  Attempting to send 29 sensor data instances. Remaining free memory (bytes): 255270552
03/12 16:02:28  Successful send to http://dasha.ics.hawaii.edu:9876/sensorbase/
03/12 16:02:28  Quitting SensorShell started at: Wed Mar 12 16:02:26 GMT-10:00 2008
03/12 16:02:28  Total sensor data instances sent: 29
```

In this case, you can see from the log file exactly when data was sent, and what the heap size was during execution.

This log file also provides another important piece of information: the total number of seconds required to transmit each batch of sensor data.  Each timestamped line that begins with "Attempting to send" is written out directly before a send to the server is attempted.

Afterwards, if successful, a line starting with "Successful send to" is printed out.  Subtracting the two timestamps from each other indicates how long the send took. In the above case, sending 29 sensor data instances took less than a second.
### 2.5 Where to go from here ###

Although each case will be different, you will want to be checking the following:
  * Is the heap space sufficient on both client and server sides?
  * Is the failure due to a timeout?  If so, can you fix the problem by simply increasing the values of the sensorshell.timeout and sensorshell.timeout.ping properties?
  * Do you see differences in behavior when you use the default embedded Derby database vs. your custom database?   If so,you might need to tune your database implementation.
  * Is the problem reproducible in the same way each time?  If not, then it may have something to do with external network loads and/or failures.
  * Does the amount of time required to send a batch of instances stay constant during the run, or does it take longer and longer as the run goes on?