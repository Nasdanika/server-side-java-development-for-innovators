# Documentation System

## Bundles

The documentation system collects runtime information about OSGi bundles and components and presents it in the Table Of Content (to be precise - the Tree Of Content, but it still abbreviates to TOC). 

The application bundles in the tree are grouped by their common prefix for the ease of navigation. 

If a bundle hosts service components, component nodes are mounted as bundle node children:

![Bundles TOC](bundles-toc.png)

A mouse click on a bundle node opens bundle documentation in the content container on the right of the content tree:

![Bundle Documentation Overview](bundle-documentation-overview.png)

Bundle documentation displays the bunle path in the content tree on the top several tabs described below. Custom tabs can be added with ``bundle-toc`` element of ``org.nasdanika.toc`` extension point.

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

Similarly to the bundle documentation, service component documentation is organized into several tabs. 
Custom tabs can be added with ``component-toc`` element of ``org.nasdanika.toc`` extension point.

### Overview 

This tab displays general component information with links to provided services and implementation class JavaDocs.

![component-documentation-overview](component-documentation-overview.png)

### Properties

This tab lists component properties with JavaDoc links to property types:

![component-documentation-properties](component-documentation-properties.png)

### References

If a service component references other components, this tab is displayed and provides information about references:

![component-documentation-references](component-documentation-references.png)

### Referenced by

This tab lists components referencing a given component:

![component-documentation-references](component-documentation-references.png)

### Context diagram

The context diagram tab provides a graphical representation of component relationships. 

![component-documentation-context-diagram](component-documentation-context-diagram.png)

The diagram can be customized via toolbar controls:

* Direction - relationship direction - both, in, or out.
* Depth - how many levels of relationships to include.
* Types - service typ names - hidden, short, or fully qualified.
* Left-to-right changes layout direction of the diagram.
* Fit width - diagram is scaled to the viewport width.
* Filter - opens a dialog to set bundle include/exclude filters, e.g. to show only the application's or ogranization's bundles. Default include/exclude filters can configured through the documentation route properties. 

## Packages

mounting

context UML diagrams

## Custom content

extensions - resolvers, renderers, ...

DSL

## User Stories and Test Results


## Search

TODO - search, link to Lucene docs.

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

