# Automated Build

By now we know how to build our application locally, now we will automate this process by publishing the code to GitHub, setting a CircleCI build/test job and Docker Hub automated build. 

In your situation you might have to use different version control and build systems and for this reason
I do not provide very detailed descriptions these particular three. 

## GitHub

Publishing to GitHub is a rather straightforward process. This is how it was done for our application:
* I created a repository on GitHub with ``README.md`` file - https://github.com/Nasdanika/bank, and copied repository URL to clipboard.
* Then I opened "Git Repositories" view in Eclipse, clicked the icon "Clone a Git Repository...", and completed the clone wizard.
* Then for each project in the workspace I right-clicked on it, selected Team > Share project, and added the project to the new Git repository.
* Finally, I've marked ``target`` and ``bin`` folders as ignored, added other files to the list of staged changes, committed them and pushed to upstream. 

## CircleCI

I already had an account on CircleCI. To build our application I had to add ``circle.yml`` file to the root of the repository:

```yml
machine:
  java:
    version: openjdk8
    
dependencies: 
  override:
    - echo 'Skipping Dependency Check'

test:
  pre:
    - wget http://nasdanika.org/winstone-0.9.10.jar
    - cd org.nasdanika.bank.aggregator && mvn package javadoc:aggregate
    
  override:
    - cd org.nasdanika.bank.aggregator && mvn verify
    
  post:
    - java -jar winstone-0.9.10.jar --webroot=org.nasdanika.bank.repository/target/repository --controlPort=8081:
        background: true
    - sleep 5
    - cd org.nasdanika.bank.product.parent && mvn package
    - java -cp winstone-0.9.10.jar winstone.tools.WinstoneControl shutdown
    
deployment:
  staging:
    branch: master
    commands:
    - mv org.nasdanika.bank.product/target/products/org.nasdanika.bank.product-*.zip $CIRCLE_ARTIFACTS    
```

You can see that build steps in the file mirror the manual build steps which we performed in previous sections. This edition of the file publishes zipped products to $CIRCLE_ARTIFACTS. In the next section we will modify the file to set up integration with Docker Hub.

## Docker Hub

TODO.
