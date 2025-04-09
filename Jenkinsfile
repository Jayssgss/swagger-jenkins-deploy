pipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *') // Poll every 5 minutes
    }
    
    stages {
        stage('Prepare') {
            steps {
                script {
                    // Clone the main repo using the deployment key
                    sshagent(['swagger-jenkins-deploy-key']) {
                        sh 'git clone git@github.com:Jayssgss/swagger-jenkins.git'
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                dir('swagger-jenkins') {
                    sh 'docker build -t swagger-jenkins:latest .'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                docker stop swagger-jenkins || true
                docker rm swagger-jenkins || true
                docker run -d --name swagger-jenkins -p 8080:8080 swagger-jenkins:latest
                '''
            }
        }
    }
}