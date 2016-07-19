# Summary

Well, it was a long enough chapter. We had only one piece of information as an input - "a web/mobile banking application" and as an output we have an application with no business functionality, but with a documentation system, including test results and JavaDoc with linked sources, and an automated build process which produces an Eclipse product and a Docker image.

One thing worth noticing about the JavaDoc is that it has not only sources linked but also dependency libraries, e.g. Eclipse and Selenium API documentation. In the case of enterprise development it may also link API documentation of internal frameworks and libraries leveraged by the application. It is important for two reasons:
* People don't have to search for API documentation - it saves time.
* When people search for API documentation there is a chance that they may find a wrong version. E.g. the application may use Eclipse Neon API's, but Google search may return Luna's JavaDoc links. So linking dependencies JavaDocs not only saves time, but also eliminates possible confusion.  

If we had to do it over again, or if we had to generate another application, it wouldn't take a lot of time with all the prerequisites being in place.

In the [next chapter](../chapter-1-user-stories/README.md) we will start working on user stories and initial tests.  