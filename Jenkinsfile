pipeline {
    agent {
        label 'agent1'
    }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_id')
        
        
    }

    stages {
        stage('Checkout') {
            steps {
                echo  "${DOCKERHUB_CREDENTIALS_PSW}  ${DOCKERHUB_CREDENTIALS_USR}"
                echo 'Pulling Git Repository'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/YoniKap/Guitar-management']])
            }
        }
        stage('Build Image') {
            steps {
                echo 'Trying to build image'
                sh 'docker build -t yonatankaplounov/application:latest /home/workspace/Application-stuff'
            }
        }
         stage('Login to Docker Hub') {
                steps {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    echo 'Login Completed'
                }
            }
            stage('Push Image to Docker Hub') {         
                steps {                            
                    sh "docker push yonatankaplounov/application:latest"           
                    echo 'Push Image Completed'       
                }            
            }
    }

    post {
        success {
            echo 'Pipeline execution succeeded!'
           
        }
        failure {
            echo 'Pipeline execution failed!'
        }
        always {
            echo 'Cleaning up...'
            sh 'docker rmi yonatankaplounov/application:latest' 
            sh 'docker logout'
            deleteDir() 
        }
    }
}