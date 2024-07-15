pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'JDK8'
    }
    environment {
        DOCKER_HUB_NAMESPACE = 'vinzydev'
        DOCKER_HUB_REPO = 'petclinic'
    }
    stages {
        stage('Build Maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }

        stage('Copy Artifact') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${DOCKER_HUB_NAMESPACE}/${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}"
                    def customImage = docker.build(imageTag, 'docker')

                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        customImage.push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh 'rm -rf docker/*.jar'
            }
        }
    }
}
