#!groovy

String awsDeploymentUserCredentialsId = "awsDeploymentUserCredentialsId"
String awsRegion = "eu-west-1"
String awsEcrContainerRepoId = "422434854706"
String awsEcrRepoName = "bloomreach-simple-app"
String awsEcrRepoUrl = awsEcrContainerRepoId +".dkr.ecr." + awsRegion + ".amazonaws.com"
String dockerFileLocation = "content-sre-assignment/app"
Map buildObject = new HashMap()
buildObject.put("artifactId","bloomreach-simple-app")
buildObject.put("version","1.1")
String tag = buildObject.get("artifactId") + ":" + buildObject.get("version")
String tagLatest = buildObject.get("artifactId") + ":latest"

node('master') {
  cleanWs()
  stage("Clone simple app"){
    sh("git clone https://github.com/virtlabx/content-sre-assignment.git")
  }

  stage("Build docker image") {
    dir(dockerFileLocation) {
      String buildCommand = "docker build --build-arg APP_VERSION=" + buildObject.get("version") + " -t " + buildObject.get("artifactId") + ":" + buildObject.get("version") + " ."
      sh(buildCommand)
    }
  }

  stage("Push docker image To ECR"){
    sh("docker tag " + tag + " " + awsEcrRepoUrl + "/" + tag)
    sh("docker tag " + tag + " " + awsEcrRepoUrl + "/" + tagLatest)
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: awsDeploymentUserCredentialsId, secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
      withEnv(['AWS_ACCESS_KEY_ID=' + env.AWS_ACCESS_KEY_ID, 'AWS_SECRET_ACCESS_KEY=' + env.AWS_SECRET_ACCESS_KEY]) {
        sh("aws ecr get-login-password --region " + awsRegion + " | docker login --username AWS --password-stdin " + awsEcrRepoUrl)
      }
    }
    sh("docker push " + awsEcrRepoUrl + "/" + tag)
    sh("docker push " + awsEcrRepoUrl + "/" + tagLatest)
  }
  
  stage("Clean Up docker images on build node") {
    sh("docker system prune -f")
    sh("docker rmi " + tag)
    sh("docker rmi " + awsEcrRepoUrl + "/" + tag)
    sh("docker rmi " + awsEcrRepoUrl + "/" + tagLatest)
  }
}
