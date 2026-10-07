@Library("Shared-library") _

pipeline {
    agent any;

    environment {
        DOCKER_IMAGE = "faizjs/jenkins-testing"
        DOCKER_TAG = "${BUILD_NUMBER}"
      }

    stages {
        stage("Checkout") {
            steps {
                // git(
                //   url: "https://github.com/Faiz-js/jenkins-implementation",
                //   branch: "main",
                //   credentialsId: "github-credentials"
                // )
                scripts {
                    checkout("https://github.com/Faiz-js/jenkins-implementation", "main", "github-credentials")
                  }
              }
          }

        stage("Build") {
            steps {
                // sh '''
                //   docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                // '''
                scripts {
                    docker_build(env.DOCKER_IMAGE, env.DOCKER_TAG)
                  } 
              }
          }

        stage("Push") {
            steps {
                withCredentials([
                  usernamePassword(
                    credentialsId: "docker-credentials",
                    usernameVariable: "DOCKER_USERNAME",
                    passwordVariable: "DOCKER_PASSWORD"
                  )
                ]) {
                    sh '''
                      echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

                      docker push $DOCKER_IMAGE:$DOCKER_TAG

                      docker logout
                    '''
                  }
              }
          }
      }
  }
