pipeline {
    agent any
    tools {
        jdk 'JDK8'
        maven 'MVN-396'
    }
    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build image') {
            steps {
                sh 'docker build -t denisterentiev/petclinic:v1'
            }
        }

        stage('Push image docker') {
            steps {
                script {
                    withCredentials([string(credentialsID: 'docker-hub-cred', passwordVariable: 'DOCKER_PASSWORD', userVariable: 'DOCKER_USER')]) {
                        sh "docker login --username $DOCKER_USER --password $DOCKER_PASSWORD"
                        sh 'docker push denisterentiev/petclinic:v1'
                    }
                }
            }
        }
    }
}
