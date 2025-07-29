pipeline {
    agent any

    tools {
        jdk 'java-11'
        maven 'maven'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashb1117/Jendoc.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t yashb1117/yash:1 .'
            }
        }

        stage('Docker Run (optional test)') {
            steps {
                sh 'docker run -d -p 9000:8080 yashb1117/yash:1'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )]) {
                        sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push yashb1117/yash:1'
            }
        }
    }
}
