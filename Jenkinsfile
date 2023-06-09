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
            expression {
                 return env.GIT_BRANCH == "origin/main"
                   }
              }
            steps {
               sh "sudo docker build -t ${Docker_Image_Name}:${env.BUILD_NUMBER} ." 
               /* sh "sudo dockes build -t ${Docker_Image_Name}:${env.BUILD_NUMBER} ." */
                sh "sudo docker inspect ${Docker_Image_Name}:${env.BUILD_NUMBER}"
            }
        }
        stage('Docker-Image-Verify') {
            steps {
               sh 'sudo docker images'
            }
        }
        
        stage('Docker-cleanup') {
            steps {
                sh 'sudo docker rm -f \$(sudo docker ps -a -q) 2 > /dev/null || true'
            }
        }
        
        stage('Docker-deploy') {
                 input {
                    message "Approval to deploy the docker image"
                             }
            steps {
                sh "sudo docker run -itd -p 80:80 ${Docker_Image_Name}:${env.BUILD_NUMBER}"
                sh 'sudo docker ps'
            }
        }  
        
 /*    removed the clean up stage and moved it to post cleanup  
       stage('Docker-images-cleanup') {
            steps {
                sh 'sudo docker image prune -af'
            }
        } */
        
     }
    
    post {
          always {
               sh 'sudo docker images'
          }
         aborted {
            sh 'sudo docker ps'
               }
         failure {
    // One or more steps need to be included within each condition's block.
             sh 'sudo docker rm -f \$(sudo docker ps -a -q) 2 > /dev/null || true'
           }
        
          success {
    // One or more steps need to be included within each condition's block.
             sh 'curl localhost'
           }
        cleanup {
            sh 'sudo docker image prune -af'
        }
     }
}
