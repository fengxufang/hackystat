# 1.0 Introduction #

One very important kind of information collected by Hackystat is called "Sensor Data".  As its name implies, this is data that is collected by "sensors", which are small software plugins attached to tools in the development environment.

All sensor data instances are required to provide six required fields: timestamp, runtime, sensordatatype, resource, owner and tool.

In addition, sensor data instances can provide any number of optional fields which provide domain-specific information. For example, a sensor attached to an editor might want to provide information about the type of editing event that triggered the sensor.  A sensor attached to a code complexity tool might want to provide information about complexity values, and so forth.

For higher level analyses to operate efficiently and effectively, they must have some way of quickly determining which sensor data instances are relevant to them, as well as some expectation about the nature of the optional fields available.   In the same way, sensors for tools with similar purposes should use the same optional fields in the same way so that analyses can interoperate.

To enable an analysis to quickly find data of relevance, Hackystat provides the "sensordatatype" field.  When a sensor generates a sensor data instance with a particular string for the sensordatatype, it is indicating that this instance will contain a certain set of optional properties.

In prior versions of Hackystat, the set of domain-specific fields were declaratively specified and could be checked automatically at run time.   During the design of Version 8, we decided that our prior approach created burdensome levels of coupling between components, and it also made evolution of structure quite difficult to manage.   We decided with Version 8 to adopt a "convention over configuration" approach to sensor data type specification.  While the required fields would still be enforced by the system, the set of sensor data types, and the optional fields associated with them, would be managed informally.

The purpose of this document is to provide a reference for the current set of "standard" sensor data types and their "standard" optional fields.  This document does not prevent someone from creating sensors that gather information with a new sensor data type field and with new optional fields that they operate on with new nalyses of their own creation.

# 2.0 Sensor Data Structure #

Sensor Data is represented in XML.   For example:

```
<?xml version="1.0"?>
<SensorData>
 <Timestamp>2007-04-30T09:00:00.000-10:00</Timestamp>
 <Runtime>2007-04-30T09:00:00.000-10:00</Runtime>
 <Tool>Subversion</Tool>
 <SensorDataType>Commit</SensorDataType>
 <Resource>file://home/johnson/svn/Foo/build.xml</Resource>
 <Owner>http://dasha.ics.hawaii.edu:9876/sensorbase/user/hongbing@hawaii.edu</Owner>
 <Properties>
   <Property>
     <Key>TotalLines</Key>
     <Value>137</Value>
   </Property>
   <Property>
     <Key>LinesAdded</Key>
     <Value>10</Value>
   </Property>
   <Property>
     <Key>LinesDeleted</Key>
     <Value>12</Value>
   </Property>
   <Property>
     <Key>Repository</Key>
     <Value>svn://www.hackystat.org/</Value>
   </Property>
   <Property>
     <Key>RevisionNumber</Key>
     <Value>2345</Value>
   </Property>
   <Property>
     <Key>Author</Key>
     <Value>johnson</Value>
   </Property>
  </Properties>
</SensorData>
```

What you can see immediately is that a SensorData instance is comprised of seven interior elements: Timestamp,
Runtime, SensorDataType, Resource, Owner, and Properties.  The first six elements constitute "required fields" that
every SensorData instance contains. The seventh element, Properties, contains a list of key-value pairs that provides
type-specific information.

Sensor Data is transmitted and received using RESTful principles.  See [Sensor Data REST API](http://code.google.com/p/hackystat-sensorbase-uh/wiki/RestApiSpecification) for details.

You can, of course, generate and send this XML using the language of your choice. If you happen to be using Java, we have developed two libraries that can be helpful:

  * SensorBaseClient ([JavaDoc](http://hackystat-sensorbase-uh.googlecode.com/svn/javadoc/org/hackystat/sensorbase/client/SensorBaseClient.html), [Examples](http://code.google.com/p/hackystat-sensorbase-uh/wiki/WritingSensorBaseClients)) provides useful facilities for sensor data retrieval, and basic facilities for transmission.
  * SensorShell ([JavaDoc](http://hackystat-sensor-shell.googlecode.com/svn/javadoc/index.html), [UserGuide](http://code.google.com/p/hackystat-sensor-shell/wiki/UserGuide)) provides more sophisticated facilities for sensor data transmission.




# 3.0 Required Sensor Data Fields #

Every sensor data instance contains the following required fields:

| **Field**  | **Example** | **Description** |
|:-----------|:------------|:----------------|
| Timestamp  | 2007-11-07T09:11:12.247-10:00 | The time instant at which the data was collected from the tool or user, in xs:dateTime (or XMLGregorianCalendar) format. |
| Runtime    | 2007-11-07T09:11:12.247-10:00 | When multiple sensor data instances should be aggregated together, this timestamp value can be used to indicate which sensor data instances belong to a single collection. For example, consider a size tool that generates one sensor data instance per file.  The sensor for this size tool should supply the same Runtime timestamp for each sensor data instance generated during a single run of the size tool over the system.  |
| SensorDataType | DevEvent    | A string indicating the "type" of data. |
| Resource   | file:/C:/svn-google/sensordata.xsd | A URI indicating the artifact from which the sensor data was generated.  |
| Owner      | johnson@hawaii.edu | The account associated with the owner of this sensor data. Note that while tools will send this data as a simple email address, the SensorBase will return the contents of this property as a RESTful URI, such as http://dasha.ics.hawaii.edu:9876/sensorbase/users/johnson@hawaii.edu |
| Tool       | Eclipse     | The name of the tool that generated this data. |

# 4.0 Standard Sensor Data Types #

## 4.1 Sensor data type descriptions ##

The following table lists the current set of "standard" sensor data types.

| **Name** | **Example tools** | **Description** |
|:---------|:------------------|:----------------|
| Build    | Ant               | Represents the outcome of a single build of a system. |
| CodeIssue | Checkstyle, PMD, FindBugs| Represents a single file and contains the counts of all the types of errors in that file. |
| Commit   | SVN               | Represents the outcome of a successful configuration management commit event on a single file. |
| Coupling | JDepend, DependencyFinder| Represents a single file (or directory) and a measure of the coupling between that file (directory) and other files (directories). |
| Coverage | Clover, Emma      | Represents a single file and one or more measures of the coverage of test cases on the code on that file. |
| DevEvent | Emacs, Eclipse, Vim | Represents developer "behavioral events", such as the action of invoking the compiler, or opening a file for editing, or running a unit test, and so forth.  |
| FileMetric | SCLC, JavaNCSS    | Represents a single file and one or more measures of the size or complexity of that file. |
| Issue    | Bugzilla, Trac, Jira, Google Project Hosting | Represents the creation of (or change in properties to) an issue in an Issue Management System.  |
| UnitTest | JUnit             | Represents the outcome of single unit test invocation on a source file. |


## 4.2 Domain-specific sensor data fields ##

The following table lists, for each sensor data type, the set of optional fields (properties) that sensor instances of a given type typically provide.

| **SDT** | **Key** | **Example values** | **Description** |
|:--------|:--------|:-------------------|:----------------|
| Build   | Result  | 'Success', 'Failure' | (Expected) Whether the build invocation passed or failed. Must be the string 'Success' or 'Failure'. |
|         | Target  | 'compile'          | The top-level target invoked as part of the build. |
|         | Type    | 'continuous.integration'  | Indicates the type of build event, if any. |
| CodeIssue | Type`_*` | 10                 | Any property prefixed with 'Type`_`' indicates a type of code issue. For example 'Type\_Indentation' indicates an 'Indentation' code issue.  The value is the number of occurrences in the file. If there are none of these properties, then the file contained no code issues. |
| Commit  | linesDeleted | 10                 | (Expected) The number of lines deleted in this commit. |
|         | linesAdded | 10                 | (Expected) The number of lines added in this commit. |
| Coupling | Type    | 'class', 'package' | (Expected) The type of coupling being calculated. |
|         | Afferent | 10                 |  (Expected) The number of afferent couplings (number of program units that depend on this one.) |
|         | Efferent | 20                 | (Expected) The number of efferent couplings (number of program units that this one depends upon). |
| Coverage | `*_`Covered | 10                 | (Expected) The number of covered constructs of the given coverage type. For example, 'method\_Covered' indicates the number of covered methods. |
|         | `*_`Uncovered | 10                 | (Expected) The number of uncovered constructs of the given coverage type. For example, 'method\_Uncovered' indicates the number of uncovered methods. |
| DevEvent | Type    | 'StateChange'      |  (Expected)  The type of dev event. |
| FileMetric | TotalLines | 200                | (Expected) The total number of lines in this file. |
|         | `*`ComplexityList | '12, 13, 23'       | Tools such as JavaNCSS can calculate measures of complexity as well as size.  This data is stored as a comma-separated list of values.  The property name indicates the type of complexity. For example, JavaNCSS provides the 'CyclomaticComplexityList' property. |
| Issue   | IssueId | '19'               |  (Expected) The unique ID assigned by the issue management system for this issue. |
|         | Type    | 'Defect--2008-09-07T11:00:00' |  The types of the issue with the time when the type updated from previous value to this value. Values of Type include Defect, Enhancement, Task, etc. D Can be multiple properties with this key. |
|         | Status  | 'Open--2008-09-07T11:00:00' |  The status of the issue with the time when the type updated from previous value to this value. Values of Status include New, Accepted, Start, Fixed, Wontfix, etc. Can be multiple properties with this key.  |
|         | Priority | 'Critical--2008-09-07T11:00:00' |  The priority of the issue with the time when the type updated from previous value to this value. Values of Priority include Low, Medium, High and Critical. Can be multiple properties with this key.  |
|         | Milestone | '8.4--2008-09-07T11:00:00' |  The milestone of the issue with the time when the type updated from previous value to this value. Values of milestone can be any string that represent the milestone version, like 8.3. Can be multiple properties with this key.  |
|         | Owner   | 'philip@hawaii.edu--2008-09-07T11:00:00' |  The owner of the issue with the time when the type updated from previous value to this value. Values of the owner are the hackystat account of the issue owner, mapped from account of issue tracking system with the SensorShellMap. Can be multiple properties with this key.  |
| UnitTest | Name    | 'org.hackystat.TestFoo' |  (Expected) The name of the unit test. |
|         | Result  | 'Pass', 'Fail'     | (Expected) Whether the unit test passed or failed. |

# 5.0 Defining a new sensor data type #

If you wish to define a new sensor data type, it is best to come up with a set of key-value pairs that you believe represent the data adequately, then submit it to the hackystat developers list for review.  This helps guarantee that your proposed representation will work as well as possible with higher level services such as DailyProjectData.