pipeline{
    agent any
    
    environment {
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }
    
    tools{
         maven 'MAVEN_HOME'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NaveedAmanat/micro-cloud-config-server.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'sudo docker build -t naveed0004/cloud-config-server:${BUILD_NUMBER} .'
                }
            }
        }
        stage('Push Iamge to Docker Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'DOCKER_HUB_KEY', variable: 'DOCKER_HUB')]) {
                        sh 'echo login to docker hub'
                      //  sh 'sudo docker push naveed0004/cloud-config-server:${BUILD_NUMBER}'
                    }
                }
            }
        }
        stage('Trigger Manifest'){
            steps{
                build job: 'micro-cloud-config-server-artifact', parameters: [string(name: 'IMAGE_TAG', value: env.BUILD_NUMBER)]
            }
        }
    }
}

