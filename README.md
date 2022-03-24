# bloomreach-pipeline

Groovy code of jenkins pipeline for bloomreach-simple-app.

This pipeline is doing the following:

- Clone simple app repository. [https://github.com/virtlabx/content-sre-assignment.git](https://github.com/virtlabx/content-sre-assignment.git)
- Build the docker image.
- Push the docker image To AWS ECR.
- Cleanup the docker image on jenkins build master node to save the space.
- Send an email in case of the build failure. 

To run this pipeline, just click build now button! 

[https://jenkins-vault-bloomreach.com/job/Bloomreach/job/bloomreach-simple-app](https://jenkins-vault-bloomreach.com/job/Bloomreach/job/bloomreach-simple-app)

You also should specify the branch that you are using as it is a should be passed as a parameter to the pipeline.

The default is master branch, but you still can create the docker image from a differnet content-sre-assignment branch as well.

To test that, use "testing-pipeline-env" which is the branch that I create in content-sre-assignment project.
