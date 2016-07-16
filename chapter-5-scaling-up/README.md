# Scaling up

## Development 

* Teams, 
* Multiple models, routes, tests - unit, integration. 
* Collection of components (back-end stubs and/or connectors) in shared repositories, 
* Trying new technologies, e.g. new UI framework leveraging existing API's and tests. Or even side-by-side.
* Issue reporting ToolbarContributor, also an issue reporting icon to the app pages.
* New members explore the system. Team visualization or team model? - vendor/contact address
* This book - scaling up knowledge.
* Tests - store in a repository (maybe only session stats), time dimension
* Leverage parts of the solution, e.g. different target runtime - leverage tests and user stories to validate migrated functionality.

Mylyn



## Runtime

* Vertical - DB, Server/Repository, Session provider. HA/Failover - DB, repository. 
* Horizontal -  web containers, ha proxy docker container in front. DB partitioning/sharding.
* Finer grained - ECF.
* Several linked docker images (Docker compose) - the app, server, db. Sirius-based docker compose file diagram editor - import, export? Docker nodes, external nodes. Visualize docker-compose.yml at runtime - extension point, register with an extension, import into a model. Also support registration of the model file if it contains additional information, e.g. notes. Mark docker-compose.yml as derived and put a note that it was generated.
* Docker swarm. 
* Hot deployments - p2.

Running in Docker - Handling of SIGTERM - Add an application with a shutdown hook, modify startup parameters - no console, app. Start as Java, not as executable?