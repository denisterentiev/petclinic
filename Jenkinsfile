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
                sh 'docker build -t denisterentiev/petclinic:$env.BUILD_NUMBER .'
            }
        }

        stage('Push image docker') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                        sh "docker login -u $DOCKER_USER -p $DOCKER_PASSWORD"
                        sh 'docker push denisterentiev/petclinic:$env.BUILD_NUMBER'
                    }
                }
            }
        }
    }
}
