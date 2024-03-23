pipeline {
     agent any
    
    stages {

        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/kparunsagar/myPythonDockerRepo.git']]])
            }
        }
        stage('build the docker image from docker file'){
            steps{
                sh 'docker image build -t arunregistry77.azurecr.io/python:${BUILD_NUMBER} .'
            }
        }
        stage('azure login and push the docker image to acr hub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'azurecontainerregistry', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh 'docker login -u ${username} -p ${password} arunregistry77.azurecr.io'
                sh 'docker image push arunregistry77.azurecr.io/python:${BUILD_NUMBER} '
                 }
            }
        }
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8099:5001 --rm --name mypython arunregistry77.azurecr.io/petclinic:${BUILD_NUMBER}'
            }
      }
    }
    }
 }
