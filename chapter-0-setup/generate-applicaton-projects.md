# Generate Application Projects
Click File > New > Other, search for ``Nasanika`` in the wizard, or scroll down to Nasdanika. Select on the ``Nasdanika Application Workspace``. 

TODO - screenshot(s).

Resolve and set the target platform.

### Workspace Projects
TODO - overview of workspace projects
* ``org.nasdanika.bank`` - Model project, ...
* ``org.nasdanika.bank.aggregator`` - 
* ``org.nasdanika.bank.app`` - Application project, ...
* ``org.nasdanika.bank.doc`` - Documentation project, JavaDoc...
* ``org.nasdanika.bank.feature`` -  
* ``org.nasdanika.bank.parent`` - 
* ``org.nasdanika.bank.product`` - 
* ``org.nasdanika.bank.repository`` - 
* ``org.nasdanika.bank.target`` - 
* ``org.nasdanika.bank.tests`` - 
* ``org.nasdanika.bank.tests.results`` - 
* ``org.nasdanika.bank.tests.results.feature`` - 
* UI Driver
  * ``org.nasdanika.ui.driver.actors`` - Contains actor interfaces and actor factory interface. Actor interface defines...
  * ``org.nasdanika.ui.driver.actors.impl`` - 
  * ``org.nasdanika.ui.driver.pages`` - 
  * ``org.nasdanika.ui.driver.pages.impl`` - 

## 7. Run the application in Eclipse
* Open the product file, 
* Click execute,
* Open the app URL in the browser,
* Open documentation in the browser - bundles, packages, docs.
* Cleanup product - remove not needed bundles - function, sca, promise, scheduler, boxing.
* Build locally, launch, browse the initial application page and documentation, including the automated UI test result.

## 8. Dockerize
* Check-in code to GitHub
* Modify GitHub URL in the generated Docker file.
* Run the file from Eclipse using Docker tools.
* Run the new image using Eclipse Docker tools.
* Open a port on CentOS. 
* Open containerized app in a Web browser.
* Stop the container, remove.
* Push image.
* Commit Dockerfile.

## 9. Customize documentation content
* Show/hide global package, bundles, and NSD content, 
* 3 doc routes - one for developers/contributors with all the details, another for business - user stories, tests, but no bundles and global packages, the last one for techies - no global packages, select bundles, e.g. organization/application bundles and core dependencies. Subclass Doc route to display different headers and maybe to have different styles.  

## Summary

* Inputs - app name
* Outputs - running app with documentation and tests
* Next steps - user stories, actors and tests.

## References
* GitHub URL
* DockerHub URL

 
[TODO]: # (Generate .cmd/.sh files in aggregator to run Maven several times - 1. Package/verify, 2. Materialize products from a local repository including test results)
[TODO]: # (Wizard to generate a test results bundle, output results to it. The bundle is part of the product feature.)
[TODO]: # (Wizard to generate Docker file)
[TODO]: # (Global packages are shown by default)
[TODO]: # (Enhancements in the generated diagram toolbar)
[TODO]: # (JavaDoc bundle and a maven command to generate to that bundle or something like that, javadoc link to the app plugin.xml extension. The bundle shall define a servlet mapped to api-docs and serving JavaDoc resources. javadoc:aggregate) 
