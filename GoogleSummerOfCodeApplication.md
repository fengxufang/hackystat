## 1.0 Introduction ##

This page contains the text of our application to the
[Google Summer of Code](http://code.google.com/soc/2008/) program.  For more
information on Google Summer of Code, see the
[FAQ](http://code.google.com/soc/2008/faqs.html) page.  Of particular interest
is the
[Notes on Organization Selection Criteria](http://groups.google.com/group/google-summer-of-code-announce/web/notes-on-organization-selection-criteria) page.

This application is due March 12, 2008.

## 2.0 Application (draft) ##

The remainder of this page provides a copy of the [Organization Application](http://code.google.com/soc/2008/org_signup.html) page along with our proposed answers.

### 2.1 About your organization ###

**1. What is your organization's name:**

Project Hackystat

**2. What is your organization's home page:**

http://www.hackystat.org/, which redirects to http://code.google.com/p/hackystat/.

**3. Describe your organization:**

The mission of the Hackystat project is to provide a framework for collection,
analysis, visualization, interpretation, annotation, and dissemination of
software development process and product data.  Work on Hackystat began in
2001 as a research activity in the Collaborative Software Developmnt
Laboratory (http://csdl.ics.hawaii.edu/) in the Department of Information
and Computer Sciences (http://www.ics.hawaii.edu) at the University of
Hawaii (http://www.hawaii.edu).

From 2001 to 2006, the project grew a substantial code base (approximately
350,000 LOC), user community (over 800 users of the "public" Version 7
Hackystat server, with an undetermined additional number of users on
private servers), and developers (dozens of contributors to the code base
from approximately 20 academic and industry sites). A significant number of
publications occurred during this time (a partial list of which is
available at http://code.google.com/p/hackystat/wiki/Publications).  In
addition, there was one commercial, non-open source spin-off based upon a
subset of this project called Sixth Sense Analytics
(http://www.6thsenseanalytics.com/).  A summary of Project Hackystat's
release history is available at
http://code.google.com/p/hackystat/wiki/History.


**4. Why is your organization applying to participate in GSoC 2008? What do you hope to gain by participating?**

We are applying for participation because we believe Hackystat
creates exciting opportunities for students who wish to participate in
defining the next generation of software engineering technologies.

In general, our current technology thrust is to explore the potential of
integrating software development measurement and analysis into the "cloud"
of internet information services.  We are interested, for example, to
explore how posting automatically generated summaries of one's developer
behavior to micro-blogging sites such as Jaiku or Twitter can improve
developer behavior transparency in a distributed work setting, and create
more productive and efficient communication among developers.  Another
direction is the creation of a social networking plugins to frameworks like
Facebook or Orkut to provide an automatically generated "profile" of one's
software development interests and expertise, which could help create more
efficient and effective software development social networks.  Our "Ideas"
page (http://code.google.com/p/hackystat/wiki/ResearchProjects) provides
more details on these and other development directions.

We hope that by participating in GSoC, the participating students will
improve their software development skills, improve their understanding of
open source development practices and communities, and hopefully be
inspired to incorporate some aspects of empirical software engineering into
their future professional practices.  We strive to follow high quality
software development practices in Hackystat through regular use of
open source technologies. We employ continuous integration via Hudson, code
quality analysis via Checkstyle, PMD, and FindBugs, unit testing via JUnit,
code coverage via Emma, and automated builds via Ant.  We prefer, but do
not mandate, the Eclipse IDE.  Most of our code base is written in Java,
and most of our user interface is written using Wicket, but we welcome
participation from those who wish to extend the framework through other
languages and technologies.

We also hope that Hackystat will benefit from the code that they
contribute, and from new ideas that they share with us.

Finally, we hope to gain new friends and colleagues.

**5. Did your organization participate in previous GSoC years? If so, please summarize your involvement and the successes and failures of your student projects. (optional)**

We participated in Google Summer of Code for the first time in 2008.  This was a very successful experience for us. A summary of our experience, with links to student blogs and projects, is available in the Google Open Source blog at:
http://google-opensource.blogspot.com/2008/10/hackystats-first-google-summer-of-code.html.

As a result of my 2008 experiences, I produced a promotional screencast for GSoC 2009:
http://www.youtube.com/watch?v=vBRRR0BQyz0

**6. If your organization has not previously participated in GSoC, have you applied in the past? If so, for what year(s)? (optional)**

N/A

**7. What license does your project use?**

GNU v2

**8. URL for your ideas page**

http://code.google.com/p/hackystat/wiki/ResearchProjects

**9. What is the main development mailing list for your organization?**

http://groups.google.com/group/hackystat-dev

**10. Where is the main IRC channel for your organization?**

Not applicable

**11. Does your organization have an application template you would like to see students use? If so, please provide it now. (optional)**

http://code.google.com/p/hackystat/wiki/GSoCStudentApplication

**12. Who will be your backup organization administrator? Please enter their Google Account address. We will email them to confirm, your organization will not become active until they respond. (optional)**

Dan Port <dport96@gmail.com>

### 2.2 About your mentors ###

**1. What criteria did you use to select these individuals as mentors? Please be as specific as possible.**

Our criteria for mentorship are: (1) The mentor must have direct experience as a developer of the Hackystat Framework; (2) The mentor must have previous experience in a supervisory/educational role; (3) The mentor must be a currently active participant in the Hackystat project.

**2. Who will your mentors be? Please enter their Google Account address separated by commas. If your organization is accepted we will email each mentor to invite them to take part. (optional)**

Philip Johnson <philipmjohnson@gmail.com>,
Dan Port <dport96@gmail.com>,
David Nickles <david.james.nickles@gmail.com>,
Aaron Kagawa <kagawaa@gmail.com>

### 2.3 About the program ###

**1. What is your plan for dealing with disappearing students?**

When possible, we will have students working with both the mentor and other Hackystat developers on an extension or enhancement.  This will lower the risk associated with students that disappear to the project, as well as lower the risk that students will want to disappear, since they will be part of a team effort.

**2. What is your plan for dealing with disappearing mentors?**

This is extremely unlikely, but if one of the  mentors actually disappears, then the others will take over his mentorship duties.

**3. What steps will you take to encourage students to interact with your project's community before, during and after the program?**

In the past, we have facilitated student visits to Hawaii for anywhere from a week to a semester to work on the project.  This seems to have worked pretty well in terms of encouraging engagement with the community, and we attempt to do the same this summer. (We typically can't subsidize housing, but we can offer office space and a small travel allowance.)

**4. What will you do to ensure that your accepted students stick with the project after GSoC concludes?**

Be the totally coolest group of developers they've ever hung out with.  That's the reason we're all still here.





