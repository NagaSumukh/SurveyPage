pipeline {
    environment {
        DOCKERHUB_PASS = credentials('nagasumukh321!')
    }
    agent any

    stages{

        stage('Build survey page') {
            steps {
                script {
                	checkout scm
                	sh 'rm -rf *.war'
                	sh 'jar -cvf Project1-SWE.war -C WebContent/ .'
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh "docker login -u nagasumukh -p ${DOCKERHUB_PASS}"
                    def customImage = docker.build("nagasumukh/newestimg:+${BUILD_TIMESTAMP}")
                   
               }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push nagasumukh/newestimg:${BUILD_TIMESTAMP}'
                }
            }
        }

      stage('Deploying Rancher to single node') {
         steps {
            script{
               sh 'kubectl set image deployment/deploymain container-0=nagasumukh/newestimg:+${BUILD_TIMESTAMP}'
            }
         }
      }

    stage('Deploying Rancher to Load Balancer') {
       steps {
          script{
             sh 'kubectl set image deployment/deploylb container-0=nagasumukh/newestimg:+${BUILD_TIMESTAMP}'
          }
       }
    }
    }
}