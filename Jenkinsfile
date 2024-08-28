pipeline {
    agent any

    environment {
        // Set up any environment variables you need here
        EMAIL_RECIPIENT = 'gurdarshan24k@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Use Maven to build the project
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Run unit and integration tests using Maven
                sh 'mvn test'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Unit and Integration Tests: ${currentBuild.fullDisplayName}",
                        body: "Unit and integration tests completed with status: ${currentBuild.result}.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis...'
                // Use SonarQube to analyze the code
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Perform a security scan with OWASP ZAP or Snyk
                sh 'snyk test'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Security Scan: ${currentBuild.fullDisplayName}",
                        body: "Security scan completed with status: ${currentBuild.result}.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Deploy to staging server using AWS CLI or Ansible
                sh 'ansible-playbook deploy-staging.yml'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Run integration tests on the staging environment
                sh 'mvn verify'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Integration Tests on Staging: ${currentBuild.fullDisplayName}",
                        body: "Integration tests on staging completed with status: ${currentBuild.result}.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Deploy to production server using AWS CLI or Ansible
                sh 'ansible-playbook deploy-prod.yml'
            }
        }
    }

    post {
        always {
            // Send an email after the pipeline completes
            emailext (
                to: "${env.EMAIL_RECIPIENT}",
                subject: "Pipeline Completion: ${currentBuild.fullDisplayName}",
                body: "The pipeline has completed with status: ${currentBuild.result}.",
                attachLog: true
            )
        }
    }
}
