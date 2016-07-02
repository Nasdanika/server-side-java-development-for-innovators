# Build and Launch Locally

## Build

There are two ways to build an Eclipse product in Eclipse.

### Eclipse Product Export Wizard

For this we'd need to open the product file, in the Exporting section click on "Elipse Product export wizard" link, uncheck "Synchronize before exporting", enter export location and click "Finish".

TODO - resolve why complains about xml.

### Maven builds

A typical Maven/Tycho build is pretty straightforward - run ``mvn clean verify`` in the aggregator project and the build will produce an Eclipse product.

Our build process is a bit trickier because we want our product to contain JavaDoc and test results. Therefore there are two pom.xml files which will be used and each will be executed twice with different goals.

Project        | Goals                   | Comment 
---------------|:-----------------------:|:--------
aggregator     | package                 | The product parent requires the application repository even in order to execute the clean task.   
product.parent | clean                   | Cleans javadoc output directory in the doc project and test results output directory in the test results project.
aggregator     | clean javadoc:aggregate | Generates aggregated JavaDoc and puts it to the doc project.  
aggregator     | verify                  | Builds bundles, executes tests and stores them to the test results project, builds a repository. 
product.parent | package                 | Uses the repository from the previous step and doc and test results bundles to build an Eclipse product.

The first row shall be executed only if there is no repository from the previous build, e.g. we build for the first time. However, in this case we don't have to execute product.parent clean. So when we run a build for the first time we can start from the third row.   

Tycho cannot load content from local repositories and we need to expose the repository over HTTP. 
It can be done in a variety of ways, e.g. by using a local web server like Apache. 
Perhaps the simplest way is to use [Winstone Servlet Container](http://winstone.sourceforge.net/).

So, before we invoke the product.parent build for the first time we shall start Winstone. It can be done from the command line as shown below 

```console
java -jar winstone-0.9.10.jar --webroot=<workspace location>\org.nasdanika.bank.repository\target\repository --controlPort=8081 
```
 
However, a better way is to create an external tool configuration:
 
![start-winstone](start-winstone.png)
 
When we are done with the build Winstone can be shut down either manually by Ctrl-C, or by issuing the following command in the command line:
```console
java -cp winstone-0.9.10.jar winstone.tools.WinstoneControl shutdown
```
 
Once again we prefer to do everything from Eclipse and so we create another external tool configuration by duplicating the "Start Winstone" configuration and changing arguments to ``-cp <path to winstone jar> winstone.tools.WinstoneControl shutdown``.
 
![shutdown-winstone](shutdown-winstone.png) 

Winstone port collides with the application port and therefore Winstone shall be down when aggregator verify is being executed. An alternative is to change Winstone port number.
 
All these steps can be scripted, but in this section we will execute them manually - there is another section dedicated to setting up an automated build. We start from the clean workspace so the sequence would be:
* aggregator javadoc:aggregate
* aggregator verify
* Start Winstone
* product.parent package
* Shutdown Winstone

For subsequent builds the sequence would be:
* Start Winstone
* product.parent clean
* Stope Winstone
* aggregator javadoc:aggregate
* aggregator verify
* Start Winstone
* product.parent package
* Shutdown Winstone

The section below show how to create build configurations.

### product.parent clean

Here is how it can be done:
* Right click on the ``org.nasdanika.bank.product.parent`` project
* Select "Run As"
* Select "Maven clean"

![product-parent-clean](product-parent-clean.png) 

### Custom Maven builds

For the first build configuration for particular project right click on the project and select "Run As > Maven build", or use ``Alt-Shift-X, M`` hot key.

![maven-build](maven-build.png)

For subsequent build configurations right click on the project and select "Run As > Maven build...". 
 
When the configuration dialog opens, modify the launch configuration name, if needed, and enter goals.

![maven-build-configuration](maven-build-configuration.png)

Then click "Run". This has to be done only once per build configuration. In subsequent builds just select a run configuration from the dropdown list:

![relaunch](relaunch.png)


## Launch

