pipeline {
    agent any;

    stages {
        stage("Checkout") {
            steps {
                git(
                  url: "https://github.com/Faiz-js/jenkins-implementation",
                  branch: "main",
                  credentialsId: "github-credentials"
                )
              }
          }

        stage("Build") {
            steps {
                sh '''
                  docker build -t faizjs/jenkins-testing:${BUILD_NUMBER} .
                '''
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
                      echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_PASSWORD --password-stdin"

                      docker push faizjs/jenkins-testing:"${BUILD_NUMBER}"

                      docker logout
                    '''
                  }
              }
          }
      }
  }
