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
 - create this simple pipeline `fly set-pipeline -t test -c resources.yml -p simple`

# Open problems
 - The unit tests are run in the following container: `1science/sbt` How can I specify a persistant storage for the ivy-dependencies ?

 - What is the right tool to release to github? I try to use two things here but I am not sure which one is correct
 - albeit running the unit tests locally seems to work successfully, I only geht the following error in the pipeline: `config path 'unit.yml' does not specify where the file lives`


# Open questions:
 - Is there a simple way to deploy as docker-containers maybe using dicker-compose?
 -  Is the grouping supposed to be used like this e.g. to include a final overview?


 # Open TODOs
 - try deployment
 - try docker container export