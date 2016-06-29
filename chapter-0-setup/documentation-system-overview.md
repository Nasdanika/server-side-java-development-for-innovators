# Documentation System

## Bundles

The documentation system collects runtime information about OSGi bundles and components and presents it in the Table Of Content (to be precise - the Tree Of Content, but it still abbreviates to TOC). 

The application bundles in the tree are grouped by their common prefix for the ease of navigation. 

If a bundle hosts service components, component nodes are mounted as bundle node children:

![Bundles TOC](bundles-toc.png)

A mouse click on a bundle node opens bundle documentation in the content container on the right of the content tree:

![Bundle Documentation Overview](bundle-documentation-overview.png)

Bundle documentation displays the bunle path in the content tree on the top several tabs described below. Custom tabs can be added with ``component-toc`` element of ``org.nasdanika.toc`` extension point.

### Overview

The overview tab displays the general bundle information as shown above.

### Required bundles

If the bundle requires other bundles they are displayed on this tab. 

![Bundle Documentation - Required Bundles](bundle-documentation-required-bundles.png)

Bundle symbolic names are rendered as links. A click on a link navigates to a respective bundle documentation.

### Headers

This tab displays a table of headers not otherwise displayed in other tabs.

![Bundle Documentation - Headers](bundle-documentation-headers.png)

### Registered Services

If the bundle has registered services - through declarative components or by other means - they are displayed in this tab.

![Bundle Documentation - Registered Services](bundle-documentation-registered-services.png)

Service types and properties types are rendered as links to JavaDoc. Component names, if a service was exposed by a DS component, are rendered as links to component documentation. Using bundles are also rendered as links.

### Services In Use

If the bundle uses any services, they are displayed in this tab. The services in use table is very similar to the registered services table.

[Bundle Documentation - Services In Use](bundle-documentation-services-in-use.png)

### Context diagram 

The context diagram tab provides a graphical representation of bundle relationships:

![Bundle Documentation - Context diagram - default](bundle-documentation-context-diagram-default.png)
 
The diagram can be customized using the toolbar:

* Direction - relationship direction - both, in, or out.
* Depth - how many levels of relationships to include.
* Dependencies - whether to show dependencies (required bundles). Dependencies are shown as dashed lines.
* Services - whether to show services exposed/referenced. Service relationships are shown as solid lines.
* Components - whether to display bundle components.
* Types - service typ names - hidden, short, or fully qualified.
* Left-to-right changes layout direction of the diagram.
* Fit width - diagram is scaled to the viewport width.
* Filter - opens a dialog to set bundle include/exclude filters, e.g. to show only the application's or ogranization's bundles. Default include/exclude filters can configured through the documentation route properties. 

![bundle-documenation-context-diagram-filter-bundles-dialog](bundle-documenation-context-diagram-filter-bundles-dialog.png)

The diagram below displays bundle components and service relationships with fully qualified service interface names:

![bundle-documentation-context-diagram-components](bundle-documentation-context-diagram-components.png)

## Components

## Packages

mounting

context UML diagrams

## Custom content

extensions - resolvers, renderers, ...

DSL

## User Stories and Test Results


## Search

## Different documentation configurations for different audiences

contributors, business people, technical people - table?

Content           | Contributors | Business | Technology 
------------------|:------------:|:--------:|-------------
Bundles           | x            |          | Filtered to organization/applications bundles.
Custom content    | x            | Filtered to organization/applications bundles.  | Filtered to organization/applications bundles.
Global packages   | x            |          | 
Session packages  | x            | Filtered to organization/application packages. | Filtered to organization/application packages.  
User Stories      | x            | x         | x
Test Results      | x            | x         | x
 

## Summary


TODO - search, link to Lucene docs.

Imagine that our boss told us about an upcoming project to build a prototype of an online/mobile banking self-service application.
He informed us that there will be a meeting set up in a day or so to talk to the business folks and clarify requirements. 
So at this point of time all information we have is a potential application name - ``bank`` or ``self-service``. 
Let's say we've decided to stick with ``bank``. 

So, what do we do at this stage with such limited information available? We will:

* [Install prerequisites](install-prerequisites.md):
  * Java, 
  * Eclipse, 
  * Maven Integration for Eclipse,
  * Tycho Configurator,
  * Nasdanika Application Workspace Wizard and Story Edtor,  
  * Graphviz.
* [Generate application projects](generate-application-projects.md)
* [Run the application in Eclipse](run-application-in-eclipse.md)
* [Build and launch the application locally](build-and-launch-locally.md)
* [Dockerize the application](dockerize.md)
* [Set up an automated build](automated-build.md)
  * Publish code to GitHub,
  * Set up build/test job on CircleCI,
  * Set up automated image build on Docker Hub.
  


## 1. Eclipse
Download [Eclipse Modeling Tools](https://www.eclipse.org/downloads/) for your platform, if you don't have it already.

## 2. Maven/m2e

* Open Eclipse marketplace client (Help > Eclipse Marketplace... menu),

![Eclipse Marketplace Menu](eclipse-marketplace-menu.png)
 
* Search for ``Maven``,

* Scroll to "Maven Integration for Eclipse (Luna and newer) 1.5" 

![M2E in Eclipse Marketplace Dialog](m2e-in-eclipse-marketplace-dialog.png)

* Click ``Install`` and follow the installation instructions.

* Once installed and restarted - go to "Help > Check for updates" to get the latest version of m2e (1.7.0 as of time of writing)

![Check for updates](check-for-updates.png)


## 3. Tycho Configurator

* Go to Window > Preferences > Maven > Discovery and click "Open Catalog". 

![Preferences Maven Discovery](preferences-maven-discovery.png)

* Search for ``tycho``, select "Tycho Configurator", click "Finish". Follow the installation instructions.

![m2e Catalog Tycho Configurator](m2e-catalog-tycho-configurator.png)

Tycho resources:

* [Home page](https://eclipse.org/tycho/)
* [Tutorial](http://www.vogella.com/tutorials/EclipseTycho/article.html).

## 4. Nasdanika Application Workspace Wizard and Story Editor

* Open the Eclipse Marketplace Client once again.
* Search for ``Nasdanika``
* Install ``Nasdanika Application Workspace Wizard`` and ``Story Editor``

TODO - Screenshot

## 5. Graphviz   
Install [Graphviz](www.graphviz.org) - it will be used behind the scenes for generation of UML diagram depicting the structure and behavior of our system.

## 6. Generate the Application Workspace
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
