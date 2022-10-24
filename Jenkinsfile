pipeline {
    
    environment {
            DOCKERHUB_CREDENTIALS = credentials('odenli')
        }
    agent { jenknode01 { image 'python:3' } }
    stages {
        stage('Clear process') {
            steps {
                echo "-=- clear processes -=-"
                sh "docker stop \044(docker ps -a -q)"
                sh "docker rm \044(docker ps -a -q)"
            }
        } 
        stage('Get files') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/orkundenli/pyserver-assignment.git']]])
            }
        }
        stage('Build image') {
            steps {
                sh "docker build -t odenli/pyserver ."
            }
        }
        stage('Run image') {
            steps {
                sh "docker run -p 8000:80 -d odenli/pyserver"
            }
        } 
        
        stage('Push Dockerhub') {
            steps {
                sh "docker tag odenli/pyserver odenli/pyserver"
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker push odenli/pyserver"
            }
        }    
    }
}
