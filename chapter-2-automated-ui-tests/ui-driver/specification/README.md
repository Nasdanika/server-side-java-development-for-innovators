# UI Driver Specification

We already have a story model with users, stories, scenarios, steps. 
With this information creation of the UI Driver specification and tests is rather straightforward. 

We will create:

* Guest and Customer actors for Guest and Customer users.
* Pages for states.   
* Test classes for stories.
* Test methods for scenarios.
* Actor/page methods for steps.

We will use ``@Link`` annotation to establish traceability between the test/UI driver classes and interfaces and the story model elements. 
In order to link UI driver and tests classes and methods to story model elements we will need to assign ID's to the referenced story model elements.

### Actors



#### Guest

#### Customer


### Pages

...

### Tests

...  



JavaDoc links to actor interfaces from story users. Bi-directional link to test results actors.

page -> state
actor -> user
test method -> scenario
actor/page method -> step


Mention Mockito or use it???