# Conclusion


## Debugging, fixes, and contributing back

Clone the server repository (URL), set the target platform from the repository project. Run your app, make fixes, create a pull request.
You may need to remove the running platform bundles.

Coin metaphor - business and technology side - what and how.

entity-relationship, component-connector.



## Experimental/planned functionality overview

A separate chapter?

Core idea - contextual execution, transparent persistence.

CDO/NFS programming model - enter context - do stuff - leave it in a consistent state ... JavaScript event loop!

CDO adds concurrency - locks on object trees, persistency, and a unique URL for every object in the repository - callbacks.

Externals - OSGi services, enroll in XA transactions.   

* Boxing - services "monikers" serialization.
* Scheduler
* Function
* Promises - unified approach - "invoke" people and third parties.
* Rules - transaction listeners (deltas), EContentAdapter, low-level Java coding or XText-based language with built-in app domain model, translates to a model - interpreted or as a source for generation. Hot deployments of rule-sets with p2. Join collections, time windows, compensating/undo rules - when the condition no longer holds.
* SCA - template model, instances in the repository, promised execution?
* xa transactions - CDO plus services. Atomikos.