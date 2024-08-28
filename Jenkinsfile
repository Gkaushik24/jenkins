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
                echo 'Building the code using Maven...'
                echo 'Command: mvn clean package'
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Mockito...'
                echo 'Command: mvn test'
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code using SonarQube...'
                echo 'Command: mvn sonar:sonar'
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency-Check...'
                echo 'Command: dependency-check.sh --project my-project --scan .'
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging server...'
                echo 'Command: scp target/my-app.jar ec2-user@staging-server:/path/to/deploy'
                echo 'Command: ssh ec2-user@staging-server "java -jar /path/to/deploy/my-app.jar &"'
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                echo 'Command: mvn verify -Denv=staging'
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production server...'
                echo 'Command: scp target/my-app.jar ec2-user@production-server:/path/to/deploy'
                echo 'Command: ssh ec2-user@production-server "java -jar /path/to/deploy/my-app.jar &"'
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
    }
}
