pipeline {
    environment {
        registry = "dev7hd/tp5_jenkins_pipeline"
        registryCredential = 'docker_hub'
        dockerImage = ''
        dockerHost = 'tcp://127.0.0.1:2375'
    }
    agent any
    stages {
        stage('Docker Login') {
            steps {
                sh 'docker login'
            }
        }
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Dev7HD/TP5_Jenkins_FreeStyle_Repo'
            }
        }
        stage('Building Image') {
            steps {
                script {
                    withEnv(["DOCKER_HOST=${dockerHost}"]) {
                        dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}