pipeline {
    agent any

    tools {
        // Make sure Maven is configured in your Jenkins
        maven 'Maven' // This must match the name of the Maven installation configured in Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Clean and package the application
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Run Maven test phase
                sh 'mvn test'
            }
            post {
                always {
                    // Archive the generated Surefire reports
                    archiveArtifacts artifacts: 'target/surefire-reports/*.xml', fingerprint: true
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy your application
                echo 'Deploying Application'
            }
        }
    }

    post {
        always {
            // Additional steps like sending notifications or cleaning up
            echo 'Build Completed'
        }
    }
}
