# User Interface

why Knockout, why not Angular - too complex, swallows/hides binding errors. Knockout conceptually is similar to JFace data binding

* Concept of routes
* Dispatch Routes, mounting of resources. Contextual execution.
* Run in Eclipse - routes and API's in documentation and in browser
* Tests - pages. Referencing of test result objects webtest-result>qualified name, webtest-actor>qualified name, webtest-page>qualified name
* Build
* Browse results. TODO - sequence diagrams and maybe SWF generation for test results
* Mobile
  * Mobile browser - responsive.
  * Hybrid. Subclassing routes to reuse common elements, or agent-based switch. 
  * Native - make API calls.
* Mobile testing
  * Mobile browser - same page classes, maybe modifications in logic to address responsiveness.
  * Hybrid, native - different page classes, but same actors and tests.
* WebSocket example - long running operation with progress feedback.
* Security

[TODO]: # (Story-based entitlements???)

Dynamic proxy NOP_DRIVER to PageFactory – is it worth the trouble?
Tests - @Service to get access to CDO transaction to validate balance?
Rendering:
- Building - HTML library
- Templating - JET - link to Vogella tutorial, old, good, compiles to Java, Java syntax, tool that works, we are not after fancy.
- Interpolation - HTML, mustache syntax `` { { } } `` - chart example from Doc route - script and how it is interpolated (Java code)
- Mix and match - again the chart example and some example with JET.
- Other things - mustach for Java, FreeMarker.

Add registration - show that spending time on sketches wireframes and mockups is not worth it because the domain model and routes are already in place. Maybe mockups, but in Guest routes.

## Additional Resources

YouTube videos showing how to create web routes using annotated EOperations. This approach is complementary to the object routes approach which we used for our application. It introduces UI concerns into the model itself and adds a dependency on web bundles to the model bundle. 
Therefore, is suitable for situations where the model's primary purpose is to serve web content and the UI is simple, 
i.e. the number of routes per class is small.
In this case keeping UI concerns in the model may make the system easier to understand.  

* [Web functionality through route operations ](https://www.youtube.com/watch?v=SdVgV-iE0Zs).
* [Bootstrap/Backbone router application ](https://www.youtube.com/watch?v=hwcMSwHS7Ew). 