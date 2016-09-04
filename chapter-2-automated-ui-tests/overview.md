# Overview

In this chapter we are going to create automated UI tests for our application, execute them and link test results to the story model elements.
 
You may wonder how are going to execute tests if we don't have our application yet. We will use [Nasdanika WebTest](https://github.com/Nasdanika/server/wiki/webtest) framework and will execute tests in a variety of ways. In particular, we will create several implementations of the UI driver:

* Sketches - this implementation will use ``@Sketch`` annotation and sketch images we already have from the previous chapter.
* Wireframes - this implementation will demonstrate how to use [PlantUML Salt](http://plantuml.com/salt.html) for wireframing.
* Mockups - this implementation will drive mock-up screens. Later we will reuse some logic from the mock-ups in the reference implementation.

Keep in mind that our primary goal is to build/define experience, not the application. As such the initial goal of our
tests is to specify that experience and then to ensure that whatever they test delivers it. 

Once fleshed out, our tests will serve as an executable specification for our application, playing a role similar to [Technology Compatibility Kit (TCK)](https://en.wikipedia.org/wiki/Technology_Compatibility_Kit) in the [Java Community Process](https://en.wikipedia.org/wiki/Java_Community_Process).
