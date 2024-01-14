pipeline {
    agent any
    environment {
        registry = "377385272712.dkr.ecr.ap-south-1.amazonaws.com/jenkins-pipeline-build-demo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Ayushi26Bhagat/Jenkins-pipeline.git']]])     
            }
        }

        stage('Building image') {
            steps{
              script {
                dockerImage = docker.build registry
              }
            }
          }
          
          stage('Pushing to ECR') {
            steps{  
                script {
                       sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 377385272712.dkr.ecr.ap-south-1.amazonaws.com'
                       sh 'docker push 377385272712.dkr.ecr.ap-south-1.amazonaws.com/jenkins-pipeline-build-demo:latest'
                }
            }
        } 

        stage('pull and Run') {
            steps{
                script {
                    sh 'docker pull 377385272712.dkr.ecr.ap-south-1.amazonaws.com/jenkins-pipeline-build-demo:latest'
                    sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 377385272712.dkr.ecr.ap-south-1.amazonaws.com/jenkins-pipeline-build-demo:latest'

                }
            }
        }
    }
    
}

