[Applying Case-Based Reasoning for Building Autonomic Service-Oriented Systems](http://csdl.ics.hawaii.edu/techreports/09-17/09-17.pdf), Herve Weitz, MS Thesis, University of Limerick, September, 2009.

This dissertation discusses autonomic computing in service-oriented
computing. We present a framework that builds a foundation for
self-healing, self-reconfiguration, self-optimization and self-protecting
service-oriented systems. We apply and implement the framework to
Hackystat, an Open Source Software developed at University of
Hawaii. Furthermore, we discuss the role of service-oriented computing in
autonomic computing, which plays a fundamental role for the relationship
between autonomic elements.  We implemented an open source
framework called Hackystat Service Manager for achieving an autonomic
service-oriented architecture in Hackystat.

[Linked Data applied to Collaborative Software Development: A case study of Hackystat](http://csdl.ics.hawaii.edu/techreports/09-16/09-16.pdf), Myriam Leggieri, MS Thesis, University of Bari, December, 2009.

This thesis investigates a new way to take advantage of RDF metadata to
support Collaborative Software Development. RDF metadata helps developers
overcome typical problems in iterative software development, such as:
exceptions thrown at run-time; making design and implementation decisions
within previously unknown domains; and usage of previously unknown tools or
libraries. Solutions usually consist of searching for suggestions from
forum posts, source code of similar projects, direct contact with specific
experts, etc.  The main problems with this approach are the time wasted in
manually detecting the searched info from unstructured documents, the low
effectiveness of search engines, and the lack of information about the
actual expertise of the directly contacted people.

In contrast, having info about projects and issues semantically structured
with RDF metadata can speed up detection of the searched details.  Dynamic
creation of RDF links with external similar RDF metadata allows users to
avoid searching or analyzing search results.  Finally, metadata about users
including quality measures coming from a trustworthy source such as
Telemetry can allow the user to trust the actual developer's
expertise. Such RDF metadata and links, together with HTTP URIs, is
provided by the Hackystat LinkedSensorData (LiSeD) service.


[We need more coverage, stat!  Classroom experiences with the Software ICU](http://csdl.ics.hawaii.edu/techreports/09-02/09-02.pdf), Philip M. Johnson and Shaoxuan Zhang, Proceedings of the 2009 International Symposium on Empirical Software Engineering and Measurement, Orlando Florida, October, 2009.

For empirical software engineering to reach its fullest potential, we must
develop effective, experiential approaches to learning about it in a
classroom setting.  In this paper, we report on a case study involving a
new approach to classroom-based empirical software engineering called the
``Software ICU''.  In this approach, students learn about nine empirical
project ``vital signs'' and use the Hackystat Framework to put their
projects into a virtual ``intensive care unit'' where these vital signs can
be assessed and monitored.  We used both questionnaire and log data to gain
insight into the strengths and weaknesses of this approach. Our evaluation
provides both quantitative and qualitative evidence concerning the overhead
of the system; the relative utility of different vital signs; the frequency
of use; and the perceived appropriateness outside of the classroom
setting. In addition to benefits, we found evidence of measurement
dysfunction induced directly by the presence of the Software ICU. We
compare these results to case studies we performed in 2003 and 2006 using
the Hackystat Framework but not the Software ICU.  We use these findings to
orient future research on empirical software engineering both inside and
outside of the classroom.

[Experiences with Hackystat as a service-oriented architecture](http://csdl.ics.hawaii.edu/techreports/09-07/09-07.pdf), Philip M. Johnson, Shaoxuan Zhang, and Pavel Senin.

Hackystat is an open source framework for automated collection and analysis
of software engineering process and product data.  Hackystat has been in
development since 2001, and has gone through eight major architectural
revisions during that time.  In 2007, we performed the latest architectural
revision, whose primary goal was to reimplement Hackystat as a
service-oriented architecture (SOA).  This version has now been in
public release for a year, and this paper reports on our experiences:
the motivations that led us to reimplement the system as a SOA, the
costs and benefits of that conversion, and our lessons learned.

[Operational Definition and Automated Inference of Test-Driven Development with Zorro](http://csdl.ics.hawaii.edu/techreports/09-01/09-01.pdf), Hongbing Kou, Philip M. Johnson and Hakan Erdogmus, Automated Software Engineering, 2009.

Test-driven development (TDD) is a style of development named for its most
visible characteristic: the design and implementation of test cases prior
to the implementation of the code required to make them pass. Many claims
have been made for TDD: that it can improve implementation as well as
design quality, that it can improve productivity, that it results in 100\%
coverage, and so forth.  However, research to validate these claims has
yielded mixed and sometimes contradictory results.  We believe that at
least part of the reason for these results stems from differing
interpretations of the TDD development style, along with an inability to
determine whether programmers actually follow whatever definition of
TDD is in use.

Zorro is a system designed to automatically determine whether a developer
is complying with an operational definition of Test-Driven Development
(TDD) practices.  Automated recognition of TDD can benefit the software
development community in a variety of ways, from inquiry into the ``true
nature'' of TDD, to pedagogical aids to support the practice of test-driven
development, to support for more rigorous empirical studies on the
effectiveness of TDD in both laboratory and real world settings.

This paper describes the Zorro system, its operational definition of TDD,
the analyses made possible by Zorro, and two empirical evaluations of the
system.  Our research shows that it is possible to define an operational
definition of TDD that is amenable to automated recognition, and
illustrates the architectural and design issues that must be addressed in
order to do so.  Zorro has implications not only for the practice of TDD,
but also for software engineering ``micro-process'' definition and
recognition through its parent framework, Software Development Stream
Analysis.


[Requirement and Design Trade-offs in Hackystat: An in-process software engineering measurement and analysis system](http://csdl.ics.hawaii.edu/techreports/06-06/06-06.pdf), Philip M. Johnson, Proceedings of the 2007 International Symposium on Empirical Software Engineering and Measurement, Madrid, Spain, September, 2007.

For five years, the Hackystat Project has incrementally developed and evaluated a generic framework for in-process software engineering measurement and analysis (ISEMA). At least five other independent ISEMA system development projects have been initiated during this time, indicating growing interest and investment in this approach by the software engineering community. This paper presents 12 important requirement and design tradeoffs made in the Hackystat system, some of their implications for organizations wishing to introduce ISEMA, and six directions for future research and development. The three goals of this paper are to: (1) help potential users of ISEMA systems to better evaluate the relative strengths and weaknesses of current and future systems, (2) help potential developers of ISEMA systems to better understand some of the important requirement and design trade-offs that they must make, and (3) help accelerate progress in ISEMA by identifying promising directions for future research and development.

[Ultra-automation and ultra-autonomy for software engineering management of ultra-large-scale systems](http://csdl.ics.hawaii.edu/techreports/07-03/07-03.pdf), Philip M. Johnson, Proceedings of the 2007 Workshop on Ultra Large Scale Systems, Minneapolis, Minnesota, May, 2007.

"Ultra-Large-Scale Systems: The Software Challenge of the Future" identifies "Engineering Management at Large Scales" as an important focus of research. Engineering management for software typically involves measurement and monitoring of products and processes in order to maintain acceptable levels of important project characteristics including cost, quality, usability, performance, reliability, and so forth. Our research on software engineering measurement over the past ten years has exhibited a trend towards increasing automation and autonomy in the collection and analysis of process and product measures. In this position paper, we extrapolate from our work so far to consider what new forms of automation and autonomy might be required for software engineering management of ULS systems.


[Automated Recognition of Test-Driven Development with Zorro](http://csdl.ics.hawaii.edu/techreports/06-13/06-13.pdf), Philip M. Johnson and Hongbing Kou, Proceedings of Agile 2007, August, 2007.

Zorro is a system designed to automatically determine whether a developer is complying with an operational definition of Test-Driven Development (TDD) practices. Automated recognition of TDD can benefit the software development community in a variety of ways, from inquiry into the "true nature" of TDD, to pedagogical aids to support the practice of test-driven development, to support for more rigorous empirical studies on the effectiveness of TDD in both laboratory and real world settings. This paper introduces the Zorro system, its operational definition of TDD, the analyses made possible by Zorro, and our ongoing efforts to validate the system.

[Improving Software Development Process and Product Management with Software Project Telemetry](http://csdl.ics.hawaii.edu/techreports/06-05/06-05.pdf), Qin Zhang, University of Hawaii, Department of Information and Computer Sciences, Ph.D. Thesis, December, 2006.

Software development is slow, expensive and error prone, often resulting in products with a large number of defects which cause serious problems in usability, reliability, and performance. To combat this problem, software measurement provides a systematic and empirically-guided approach to control and improve software development processes and final products. However, due to the high cost associated with "metrics collection" and difficulties in "metrics decision-making," measurement is not widely adopted by software organizations.

This dissertation proposes a novel metrics-based program called "software project telemetry" to address these problems. It uses software sensors to collect metrics automatically and unobtrusively. It employs a domain-specific language to represent telemetry trends in software product and process metrics. Project management and process improvement decisions are made by detecting changes in telemetry trends and comparing trends between different periods of the same project. Software project telemetry avoids many problems inherent in traditional metrics models, such as the need to accumulate a historical project database and ensure that the historical data remain comparable to current and future projects.

The claim of this dissertation is that software project telemetry provides an effective approach to (1) automated metrics collection and analysis, and (2) in-process, empirically-guided software development process problem detection and diagnosis. Two empirical studies were carried out to evaluate the claim: one in software engineering classes, and the other in the Collaborative Software Development Lab. The results suggested that software project telemetry had acceptably-low metrics collection and analysis overhead, and that it provided decision-making value at least in the exploratory context of the two studies.

[Automated recognition of low-level process: A pilot validation study of Zorro for test-driven development](http://csdl.ics.hawaii.edu/techreports/06-02/06-02.pdf), Hongbing Kou and Philip M. Johnson, Proceedings of the 2006 International Workshop on Software Process, Shanghai, China, May, 2006.

Zorro is a system designed to automatically determine whether a developer is complying with the Test-Driven Development (TDD) process. Automated recognition of TDD could benefit the software engineering community in a variety of ways, from pedagogical aids to support the learning of test-driven design, to support for more rigorous empirical studies on the effectiveness of TDD in practice. This paper presents the Zorro system and the results of a pilot validation study, which shows that Zorro was able to recognize test-driven design episodes correctly 89\% of the time. The results also indicate ways to improve Zorro's classification accuracy further, and provide evidence for the effectiveness of this approach to low-level software process recognition.

[Actual Process: A Research Program](http://csdl.ics.hawaii.edu/techreports/06-01/06-01.pdf), Lutz Prechelt and Sebastian Jekutsch and Philip M. Johnson.

Most process research relies heavily on the use of terms and concepts whose validity depends on a variety of assumptions to be met. As it is difficult to guarantee that they are met, such work continually runs the risk of being invalid. We propose a different and complementary approach to understanding process: Perform all description bottom-up and based on hard data alone. We call the approach actual process and the data actual events. Actual events can be measured automatically. This paper describes what has been done in this area already and what are the core problems to be solved in the future.


[Results from the 2006 Classroom Evaluation of Hackystat-UH](http://csdl.ics.hawaii.edu/techreports/07-02/07-02.html), Philip M. Johnson, Department of Information and Computer Sciences, University of Hawaii, Honolulu, Hawaii 96822, Number CSDL-07-02, December, 2006.

This report presents the results from a classroom evaluation of Hackystat by ICS 413 and ICS 613 students at the end of Fall, 2006. The students had used Hackystat-UH for approximately six weeks at the time of the evaluation. The survey requests their feedback regarding the installation, configuration, overhead of use, usability, utility, and future use of the Hackystat-UH configuration. This classroom evaluation is a semi-replication of an evaluation performed on Hackystat by ICS 413 and 613 students at the end of Fall, 2003, which is reported in "Results from the 2003 Classroom Evaluation of Hackystat-UH". As the Hackystat system has changed significantly since 2003, some of the evaluation questions were changed.  The data from this evaluation, in combination with the data from the 2003 evaluation, provide an interesting perspective on the past, present, and possible future of Hackystat. Hackystat has increased significantly in functionality since 2003, which has enabled the 2006 usage to more closely reflect industrial application, and which has resulted in significantly less overhead with respect to client-side installation. On the other hand, results appear to indicate that this increase in functionality has resulted in a decrease in the usability and utility of the system, due to inadequacies in the server-side user interface. Based upon the data, the report proposes a set of user interface enhancements to address the problems raised by the students, including Ajax-based menus and parameters, workflow based organization of the user interface, real-time display for ongoing project monitoring, annotations, and simplified data exploration facilities.

[Continuous GQM: An automated framework for the Goal-Question-Metric paradigm](http://csdl.ics.hawaii.edu/techreports/05-09/05-09.pdf), Christoph Lofi, Department of Software Engineering, Fachbereich Informatik, Universitat Kaiserslautern, Germany, M.S. Thesis, Number CSDL-05-09, August, 2005.

Abstract: Measurement is an important aspect of Software Engineering as it is the foundation of predictable and controllable software project execution. Measurement is essential for assessing actual project progress, establishing baselines and validating the effects of improvement or controlling actions.

The work performed in this thesis is based on Hackystat, a fully automated measurement framework for software engineering processes and products. Hackystat is designed to unobtrusively measure a wide range of metrics relevant to software development and collect them in a centralized data repository. Unfortunately, it is not easy to interpret, analyze and visualize the vast data collected by Hackystat in such way that it can effectively be used for software project control.

A potential solution to that problem is to integrate Hackystat with the GQM (Goal / Question / Metric) Paradigm, a popular approach for goal-oriented, systematic definition of measurement programs for software-engineering processes and products. This integration should allow the goal-oriented use of the metric data collected by Hackystat and increase it�s usefulness for project control. During the course of this work, this extension to Hackystat which is later called hackyCGQM is implemented. As a result, hackyCGQM enables Hackystat to be used as a Software Project Control Center (SPCC) by providing purposeful high-level representations of the measurement data.

Another interesting side-effect of the combination of Hackystat and hackyCGQM is that this system is able to perform fully automated measurement and analysis cycles. This leads to the development of cGQM, a specialized method for fully automated, GQM based measurement programs. As a summary, hackyCGQM seeks to implement a completely automated GQMbased measurement framework. This high degree of automation is made possible by limiting the implemented measurement programs to metrics which can be measured automatically, thus sacrificing the ability to use arbitrary metrics.


[Telemetry Plate Lunch Contest Results](http://csdl.ics.hawaii.edu/techreports/05-07/05-07.html), Philip M. Johnson, Department of Information and Computer Sciences, University of Hawaii, Honolulu, Hawaii 96822, Number CSDL-05-07, July, 2005.

The "Telemetry Plate Lunch Contest" was a contest to support investigation of the use of multi-axis telemetry charts in Hackystat. This document describes the winning submissions.

[A continuous, evidence-based approach to discovery and assessment of software engineering best practices](http://csdl.ics.hawaii.edu/techreports/05-05/05-05.pdf), Philip M. Johnson, Department of Information and Computer Sciences, University of Hawaii, Honolulu, Hawaii 96822, Number CSDL-05-05, June, 2005.

Abstract: This document discusses an approach that integrates Hackystat, Software Project Telemetry, Software Development Stream Analysis, Pattern Discovery, and Evidence-based software engineering to support evaluation of best practices. Both classroom and industrial case studies are proposed to support evaluation of the techniques.

[Understanding HPCS development through automated process and product measurement with Hackystat](http://csdl.ics.hawaii.edu/techreports/04-22/04-22.pdf), Philip M. Johnson and Michael G. Paulding, Second Workshop on Productivity and Performance in High-End Computing (P-PHEC), February, 2005.

The high performance computing (HPC) community is increasingly aware that traditional low-level, execution-time measures for assessing high-end computers, such as flops/second, are not adequate for understanding the actual productivity of such systems. In response, researchers and practitioners are exploring new measures and assessment procedures that take a more wholistic approach to high performance productivity. In this paper, we present an approach to understanding and assessing development-time aspects of HPC productivity. It involves the use of Hackystat for automatic, non-intrusive collection and analysis of six measures: Active Time, Most Active File, Command Line Invocations, Parallel and Serial Lines of Code, Milestone Test Success, and Performance. We illustrate the use and interpretation of these measures through a case study of small-scale HPC software development. Our results show that these measures provide useful insight into development-time productivity issues, and suggest promising additions to and enhancements of the existing measures.


[Improving Software Development Management through Software Project Telemetry](http://csdl.ics.hawaii.edu/techreports/04-11/04-11.pdf), Philip M. Johnson and Hongbing Kou and Michael G. Paulding and Qin Zhang and Aaron Kagawa and Takuya Yamashita, IEEE Software, August, 2005.

Software project telemetry is a new approach to software project management in which sensors are attached to development environment tools to unobtrusively monitor the process and products of development. This sensor data is abstracted into high-level perspectives on development trends called Telemetry Reports, which provide project members with insights useful for local, in-process decision making. This paper presents the essential characteristics of software project telemetry, contrasts it to other approaches such as predictive models based upon historical software project data, describes a reference framework implementation of software project telemetry called Hackystat, and presents our lessons learned so far.

[Practical automated process and product metric collection and analysis in a classroom setting: Lessons learned from Hackystat-UH](http://csdl.ics.hawaii.edu/techreports/03-12/03-12.pdf), Philip M. Johnson and Hongbing Kou and Joy M. Agustin and Qin Zhang and Aaron Kagawa and Takuya Yamashita, Proceedings of the 2004 International Symposium on Empirical Software Engineering, Los Angeles, California, August, 2004.

Measurement definition, collection, and analysis is an essential component of high quality software engineering practice, and is thus an essential component of the software engineering curriculum. However, providing students with practical experience with measurement in a classroom setting can be so time-consuming and intrusive that it's counter-productive---teaching students that software measurement is "impractical" for many software development contexts. In this research, we designed and evaluated a very low-overhead approach to measurement collection and analysis using the Hackystat system with special features for classroom use. We deployed this system in two software engineering classes at the University of Hawaii during Fall, 2003, and collected quantitative and qualitative data to evaluate the effectiveness of the approach. Results indicate that the approach represents substantial progress toward practical, automated metrics collection and analysis, though issues relating to the complexity of installation and privacy of user data remain.

[The Hackystat-JPL Configuration: Overview and Initial Results, Philip M. Johnson](http://csdl.ics.hawaii.edu/techreports/03-07/03-07.html), Department of Information and Computer Sciences, University of Hawaii, Honolulu, Hawaii 96822, Number CSDL-03-07, October, 2003.

This report presents selected initial results from Hackystat-based descriptive analyses of Harvest workflow data gathered from the Mission Data System software development project from January, 2003 to August, 2003. We present the motivation for this work, the methods used, examples of the analyses, and questions raised by the results. Our major findings include: (a) workflow transitions not documented in the "official" process; (b) significant numbers of packages with unexpected transition sequences; (c) cyclical levels of development "intensity" as represented by levels of promotion/demotion; (d) a possible approach to calculating the proportion of "new" scheduled work versus rework/unscheduled work along with baseline values; and (e) a possible approach to calculating the distribution of package "ages" and days spent in the various workflow states, along with potential issues with the representation of "package age" based upon the current approach to package promotion.

The report illustrates how our current approach to analysis can yield descriptive perspectives on the MDS development process. It provides a first step toward more prescriptive, analytic models of the MDS software development process by providing insights into the potential uses and limitations of MDS product and process data.

[Priority Ranked Inspection: Supporting Effective Inspection in Resource-limited Organizations](http://csdl.ics.hawaii.edu/techreports/05-01/05-01.pdf), Aaron Kagawa, Department of Information and Computer Sciences, University of Hawaii, Honolulu, Hawaii 96822, M.S. Thesis, Number CSDL-05-01, August, 2005.

Abstract: Imagine that your project manager has budgeted 200 person-hours for the next month to inspect newly created source code. Unfortunately, in order to inspect all of the documents adequately, you estimate that it will take 400 person-hours. However, your manager refuses to increase the budgeted resources for the inspections. How do you decide which documents to inspect and which documents to skip? Unfortunately, the classic definition of inspection does not provide any advice on how to handle this situation. For example, the notion of entry criteria used in Software Inspection determines when documents are ready for inspection rather than if it is needed at all.

My research has investigated how to prioritize inspection resources and apply them to areas of the system that need them more. It is commonly assumed that defects are not uniformly distributed across all documents in a system, a relatively small subset of a system accounts for a relatively large proportion of defects. If inspection resources are limited, then it will be more effective to identify and inspect the defect-prone areas.

To accomplish this research, I have created an inspection process called Priority Ranked Inspection (PRI). PRI uses software product and development process measures to distinguish documents that are "more in need of inspection" (MINI) from those "less in need of inspection" (LINI). Some of the product and process measures include: user-reported defects, unit test coverage, active time, and number of changes. I hypothesize that the inspection of MINI documents will generate more defects with a higher severity than inspecting LINI documents.

My research employed a very simple exploratory study, which includes inspecting MINI and LINI software code and checking to see if MINI code inspections generate more defects than LINI code inspections. The results of the study provide supporting evidence that MINI documents do contain more high-severity defects than LINI documents. In addition, there is some evidence that PRI can provide developers with more information to help determine what documents they should select for inspection.

[Beyond the Personal Software Process: Metrics collection and analysis for the differently disciplined](http://csdl.ics.hawaii.edu/techreports/02-07/02-07.pdf), Philip M. Johnson and Hongbing Kou and Joy M. Agustin and Christopher Chan and Carleton A. Moore and Jitender Miglani and Shenyan Zhen and William E. Doane, Proceedings of the 2003 International Conference on Software Engineering, Portland, Oregon, May, 2003.

Pedagogies such as the Personal Software Process (PSP) shift metrics definition, collection, and analysis from the organizational level to the individual level. While case study research indicates that the PSP can provide software engineering students with empirical support for improving estimation and quality assurance, there is little evidence that many students continue to use the PSP when no longer required to do so. Our research suggests that this "PSP adoption problem" may be due to two problems: the high overhead of PSP-style metrics collection and analysis, and the requirement that PSP users "context switch" between product development and process recording. This paper overviews our initial PSP experiences, our first attempt to solve the PSP adoption problem with the LEAP system, and our current approach called Hackystat. This approach fully automates both data collection and analysis, which eliminates overhead and context switching. However, Hackystat changes the kind of metrics data that is collected, and introduces new privacy-related adoption issues of its own.
