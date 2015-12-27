This page presents internal (Hackystat) and external (non-Hackystat) library dependencies for the Hackystat projects.

Note that the external dependencies listed below are "run-time" dependencies; libraries that must be present in order for the project to run successfully. In addition to these "run-time" dependencies, the Java-based Hackystat projects also have a number of "development-time" dependencies. These development-time dependencies are static analysis tools such as: JUnit, FindBugs, PMD, DependencyFinder, JavaNCSS, and SCLC.

| **Project** | **Internal Dependencies** | **External Dependencies** |
|:------------|:--------------------------|:--------------------------|
| hackystat-utilities | (none)                    | Apache JCS, util.concurrent, Apache Commons Logging |
| hackystat-sensorbase-uh | hackystat-utilities       | Restlet, Derby            |
| hackystat-sensorbase-postgres | hackystat-sensorbase-uh   | Postgres                  |
| hackystat-sensor-shell | hackystat-sensorbase-uh   | (none)                    |
| hackystat-analysis-dailyprojectdata | hackystat-sensorbase-uh   | (none)                    |
| hackystat-analysis-telemetry | hackystat-analysis-dailyprojectdata | (none)                    |
| hackystat-sensorbase-simdata | hackystat-sensor-shell, hackystat-analysis-telemetry | (none)                    |
| hackystat-sensor-ant | hackystat-sensor-shell    | SVNkit                    |
| hackystat-sensor-xmldata | hackystat-sensor-shell    | (none)                    |
| hackystat-sensor-eclipse | hackystat-sensor-shell    | (none)                    |
| hackystat-sensor-example | hackystat-sensor-ant      | (none)                    |
| hackystat-sensor-emacs | hackystat-sensor-ant      | (none)                    |
| hackystat-sensor-vim | hackystat-sensor-shell    | (none)                    |
| hackystat-ui-tickertape | hackystat-analysis-telemetry | (none)                    |
| hackystat-ui-wicket | hackystat-analysis-telemetry | Apache Wicket, Slf4J, Jetty |

