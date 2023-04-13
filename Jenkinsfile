pipeline {
    environment {
        registry = "nagasumukh/newestimg"
        registryCredential = 'Dockerhub'
        DOCKERHUB_PASS = credentials('Dockerhub')
        TIMESTAMP = new Date().format("yyyyMMdd_HHmmss")
    }
    agent any

    stages{

            stage('Build survey page') {
                steps {
                    script {
                        sh 'rm -rf *.war'
                        sh 'jar -cvf Project1-SWE.war -C src/main/webapp/ .'
                        sh 'echo ${BUILD_TIMESTAMP}'

                        docker.withRegistry('',registryCredential){
                            def customImage = docker.build("nagasumukh/newestimg:${env.TIMESTAMP}")
                        }



//                         sh "docker login -u Nagasumukh -p ${DOCKERHUB_PASS}"
//                         def customImage = docker.build("nagasumukh/newestimg:${env.TIMESTAMP}")

                   }
                }
            }

            stage('Push to Docker Hub') {
                steps {
                    script {
                        sh 'docker push nagasumukh/newestimg:${env.TIMESTAMP}'
                    }
                }
            }

          stage('Deploying Rancher to single node') {
             steps {
                script{
                   sh 'kubectl set image deployment/deploymain container-0=nagasumukh/newestimg:${env.TIMESTAMP}'
                }
             }
          }

        stage('Deploying Rancher to Load Balancer') {
           steps {
              script{
                 sh 'kubectl set image deployment/deploylb container-0=nagasumukh/newestimg:${env.TIMESTAMP}'
              }
           }
        }
    }
}
