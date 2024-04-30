pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Package the application without running tests
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                // Run only the test phase with Surefire reports being generated
                sh 'mvn -B test'
            }
            post {
                always {
                    // Archive the Surefire reports
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', fingerprint: true
                    // Publish test results to Jenkins dashboard
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('pmd') {
            steps {
                // Run PMD analysis
                sh 'mvn pmd:pmd'
            }
        }
    }

    post {
        always {
            // Archive site contents including PMD reports
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            // Archive all generated JAR and WAR files
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}
