pipeline {
    agent any
    stages {
        stage("Source code checkout"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Devendhar/ansible1']])
            }
        }
        stage("Build docker image"){
            steps{
                script{
                    sh 'podman build -t python-flask-image .'
                    sh 'podman images'
                }
            }
        }
        stage("Push docker image to JCR"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'registrypwd', variable: 'registrypwd')]) {
                        sh 'podman login -u admin -p ${registrypwd} 192.168.56.102:8082'
                    }
                    sh 'podman push localhost/python-flask-image:latest docker://192.168.56.102:8082/docker-local/python-flask-image:latest'
                }
            }
        }
    }
}
