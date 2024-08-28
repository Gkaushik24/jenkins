pipeline {
    agent any
    
    environment {
        EMAIL_RECIPIENT = 'gurdarshan24k@gmail.com'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the code using Maven...'
                sh 'mvn clean package'
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Mockito...'
                sh 'mvn test'
            }
            post {
                always {
                    script {
                        def result = currentBuild.currentResult
                        emailext(
                            to: env.EMAIL_RECIPIENT,
                            subject: "Unit and Integration Tests - ${result}",
                            body: "The Unit and Integration Tests stage has ${result}. Please check the attached logs.",
                            attachLog: true
                        )
                    }
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code using SonarQube...'
                sh 'mvn sonar:sonar'
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency-Check...'
                bat 'dependency-check.bat --project my-project --scan .'
            }
            post {
                always {
                    script {
                        def result = currentBuild.currentResult
                        emailext(
                            to: env.EMAIL_RECIPIENT,
                            subject: "Security Scan - ${result}",
                            body: "The Security Scan stage has ${result}. Please check the attached logs.",
                            attachLog: true
                        )
                    }
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging server...'
                bat '''
                pscp target\\my-app.jar ec2-user@staging-server:/path/to/deploy
                plink ec2-user@staging-server "java -jar /path/to/deploy/my-app.jar &"
                '''
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                sh 'mvn verify -Denv=staging'
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production server...'
                bat '''
                pscp target\\my-app.jar ec2-user@production-server:/path/to/deploy
                plink ec2-user@production-server "java -jar /path/to/deploy/my-app.jar &"
                '''
            }
        }
    }
    
    post {
        success {
            emailext(
                to: env.EMAIL_RECIPIENT,
                subject: "Pipeline Successful",
                body: "The Jenkins pipeline has completed successfully."
            )
        }
        failure {
            emailext(
                to: env.EMAIL_RECIPIENT,
                subject: "Pipeline Failed",
                body: "The Jenkins pipeline has failed. Please check the attached logs.",
                attachLog: true
            )
        }
    }
}
