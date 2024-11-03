pipeline {
    agent any
    stages {
        stage('Pre-cleanup (Targeted)') {
            steps {
                script {
                    try {
                        bat 'rmdir /S /Q target || echo Target directory not found'
                    } catch (Exception e) {
                        echo "Pre-cleanup failed: ${e}"
                    }
                }
            }
        }
        stage('Checkout') {
            steps {
                git 'https://github.com/MADHAVAN-BE-2003/Maven_Project_1.git'
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
            script {
                echo 'Attempting final cleanup after failure...'
                bat 'taskkill /f /im java.exe || echo No Java processes to kill'
            }
        }
    }
}
