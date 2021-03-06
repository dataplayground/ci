Getting familiar with concourse ci
===========================

This are my steps familiarizing with concourse ci. I generated the source-project at https://github.com/dataplayground/restSimple using the activator: `activator-service-container-tutorial`

Now I try to set up a simple CI / CD pipeline.

# Overview of the pipeline
The pipeline should watch https://github.com/dataplayground/restSimple for new commits and trigger tests if a new commit is detected on master.

For testing I will simply run unit-tests using `sbt test`
If the tests complete successfully, I want to deploy the result to github as a release.

https://github.com/dataplayground/ci comtains my current progress at building the pipeline.

# Steps to follow along
 - `vagrant init concourse/lite` create the Vagrantfile
 - `vagrant up` create VM + bring it up
 - CI now running at: http://192.168.100.4:8080/
 - install fly cli e.g. as described here: https://github.com/starkandwayne/concourse-tutorial
 - login as `fly --target test login  --concourse-url http://192.168.100.4:8080 sync`
 - "manually" run unit tests `fly -t test execute -c unit.yml`
 - to load credentials use parameters as such: http://concourse.ci/fly-cli.html#parameters you can use `fly set-pipeline --pipeline test --config resources.yml --load-vars-from credentials.yml
 - to see what is going on 1) connect to the container `fly intercept -j test/worker-main -s unit` 2) see the logs like: `TODO`

# Open problems
 - The unit tests are run in the following container: `1science/sbt` How can I specify a persistant storage for the ivy-dependencies ?
 - What is the right tool to release to github? I try to use two things here but I am not sure which one is correct


# Open questions:
 - Is there a simple way to deploy as docker-containers maybe using docker-compose?
 - Is the grouping supposed to be used like this e.g. to include a final overview?
 - Is it good practice to spread ci-config over the modules e.g. to move 'unit.yml' to the project? e.g. like here where CI resides in a different GIT than the actual project to be compiled.
 - How can I match any (feature) branch as a git resource?
 - How to see what is "exactly going on" if I am inside a container - where do I find the logs of sbt whilst they are streamed to the frontend?

# Open TODOs
 - try deployment
 - try docker container export to hub
 - hock into repository
 - releases
 - auto-merge feature into dev if tets pass
 - fetch only n-git revisions in resources for speedup
