pipeline {
    agent any
    
    environment {
        EMAIL_RECIPIENT = "gurdarshan24k@gmail.com"
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code using Maven...'
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests using JUnit and Mockito...'
                    sh 'mvn test'
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                script {
                    echo 'Analyzing code using SonarQube...'
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan using OWASP Dependency-Check...'
                    sh 'dependency-check.sh --project my-project --scan .'
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging server...'
                    sh 'scp target/my-app.jar ec2-user@staging-server:/path/to/deploy'
                    sh 'ssh ec2-user@staging-server "java -jar /path/to/deploy/my-app.jar &"'
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging environment...'
                    sh 'mvn verify -Denv=staging'
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production server...'
                    sh 'scp target/my-app.jar ec2-user@production-server:/path/to/deploy'
                    sh 'ssh ec2-user@production-server "java -jar /path/to/deploy/my-app.jar &"'
                }
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
                body: "The Jenkins pipeline has failed. Please check the console output for details."
            )
        }
    }
}
