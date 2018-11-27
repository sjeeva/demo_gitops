void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/sjeeva/demo_gitops"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

pipeline {
    agent { label "build" }
    stages {
         stage("checkout"){
             steps {
                checkout scm
             }
         }
         stage("build") {
             steps {
                 sh "docker build -t sampleapp ."
             }
         }
         stage("run") {
             steps {
                 echo "run stage"
                 sh "docker rm -f sampleapp"
                 sh "docker run -d -p 5000:5000 --name sampleapp sampleapp"
             }
         }
     }
    
     post {
        success {
             setBuildStatus("Build succeeded", "SUCCESS");
        }
        failure {
             setBuildStatus("Build failed", "FAILURE");
        }
    } 
}
