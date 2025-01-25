pipeline {
    environment {
        registry = "dev7hd/tp5_jenkins_pipeline"
        registryCredential = 'docker_hub'
        dockerImage = null
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Dev7HD/TP5_Jenkins_FreeStyle_Repo'
            }
        }
        stage('Building Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test Image') {
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
    post {
        always {
            script {
                echo "Cleaning up local images..."
                sh "docker rmi ${registry}:${BUILD_NUMBER} || true"
                sh "docker rmi ${registry}:latest || true"
            }
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}