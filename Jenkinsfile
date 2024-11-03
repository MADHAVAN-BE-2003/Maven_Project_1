pipeline {
    agent any
    options {
        skipDefaultCheckout()
    }
    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                git 'https://github.com/MADHAVAN-BE-2003/Maven_Project_1.git'
            }
        }
        stage('Pre-clean') {
            steps {
                bat 'rmdir /S /Q target'
            }
        }
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'MAVEN_HOME', type: 'maven'
                    bat "${mvnHome}\\bin\\mvn clean package"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    def mvnHome = tool name: 'MAVEN_HOME', type: 'maven'
                    bat "${mvnHome}\\bin\\mvn test"
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
