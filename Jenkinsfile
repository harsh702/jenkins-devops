pipeline {
    agent any
    environment {
        
        Docker_Image_Name = 'myimage'
        Docker_Tag = 'v2'
    }
    stages {
        
        stage('Pre-Checks'){
            steps{
                parllel(
                    Docker-Verify: { sh 'docker --version' },
                    Git-Verify: { sh 'git --version' }
                        )
            }
        }
        
/*        stage('Docker-Verify') {
            steps {
                sh 'docker --version'
                  }
        }
       stage('Git-Verify') {
            steps {
               sh 'git --version'
            }
        }
     */
       stage('Docker-Build') {
            steps {
                sh "sudo docker build -t ${Docker_Image_Name}:${env.BUILD_NUMBER} ."
            }
        }
        stage('Docker-Image-Verify') {
            steps {
               sh 'sudo docker images'
            }
        }
    }
}
