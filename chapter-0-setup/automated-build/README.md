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

## Building on CircleCI and publishing to Docker Hub

I already had accounts on CircleCI and Docker Hub. To build our application I had to add ``circle.yml`` file to the root of the repository:

```yml
machine:
  java:
    version: openjdk8
  services:
    - docker
        
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
    - docker build -t nasdanika/bank:$CIRCLE_BUILD_NUM org.nasdanika.bank.product/target/products/org.nasdanika.bank.product/linux/gtk
    
deployment:
  staging:
    branch: master
    commands:
    - mv org.nasdanika.bank.product/target/products/org.nasdanika.bank.product-*.zip $CIRCLE_ARTIFACTS    
    - docker login -e $DOCKER_EMAIL -u $DOCKER_USER -p $DOCKER_PASS
    - docker push nasdanika/bank:$CIRCLE_BUILD_NUM
```

You can see that build steps in the file mirror the manual build steps which we performed in previous sections. 

A docker image published by the build process is available at [Docker Hub Nasdanika Bank](https://hub.docker.com/r/nasdanika/bank/) repository. 


