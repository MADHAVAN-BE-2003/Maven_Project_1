pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MADHAVAN-BE-2003/Maven_Project_1.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3', type: 'maven'
                    withMaven(maven: mvnHome) {
                        sh 'mvn clean package'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven 3', type: 'maven'
                    withMaven(maven: mvnHome) {
                        sh 'mvn test'
                    }
                }
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }
    }
    post {
        success {
            echo 'Build and Test Succeeded'
        }
        failure {
            echo 'Build or Test Failed'
        }
    }
}
