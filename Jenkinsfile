pipeline {
    agent any
    environment {
        
        Docker_Image_Name = 'myimage'
        Docker_Tag = 'v2'
    }
    
    options {
            timestamps()
            buildDiscarder(logRotator(numToKeepStr: '10'))
            disableConcurrentBuilds()
               }
    
    stages {
        
        stage('Pre-Checks'){
            parallel {
              stage('Docker-Verify') {
                steps {
                  sh 'docker --version'
                         }
               }
              stage('Git-Verify') {
                steps {
                 sh 'git --version'
                    }
                  }
             }
     }
     
       stage('Docker-Build') {
           when {
                branch 'origin/test'
                     }
            steps {
                sh "sudo docker build -t ${Docker_Image_Name}:${env.BUILD_NUMBER} ."
                sh "sudo docker inspect ${Docker_Image_Name}:${env.BUILD_NUMBER}"
            }
        }
        stage('Docker-Image-Verify') {
            steps {
               sh 'sudo docker images'
            }
        }
    }
}
