pipeline {
    agent any

    options {
        // Retries the pipeline on SCM checkout failures and applies a timeout
        retry(3)
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                script {
                    // Clean the workspace at the start
                    deleteDir()
                    echo 'Workspace cleaned up successfully.'
                }
            }
        }
        
        stage('Checkout Code') {
            steps {
                script {
                    try {
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: '*/main']], // specify the branch name
                            userRemoteConfigs: [[url: 'https://github.com/MADHAVAN-BE-2003/Maven_Project_1.git']],
                            extensions: [[$class: 'CleanBeforeCheckout']]
                        ])
                        echo 'Code checkout successful.'
                    } catch (Exception e) {
                        error("Failed to checkout code: ${e.message}")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run Maven build command
                    bat "mvn clean install"
                    echo 'Build completed successfully.'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // Run Maven test command
                    bat "mvn test"
                    echo 'Tests executed successfully.'
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up workspace after completion
                echo 'Cleaning up workspace post-build.'
                try {
                    deleteDir()
                } catch (Exception e) {
                    echo "Failed to delete workspace: ${e.message}"
                }
            }
        }
        success {
            echo 'Build and test stages completed successfully.'
        }
        failure {
            echo 'Build or Test failed. Check logs for details.'
        }
    }
}
