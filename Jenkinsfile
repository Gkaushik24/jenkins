pipeline {
    agent any

    tools {
        // Define the tools required by your pipeline (e.g., JDK, Maven)
        maven 'Maven 3.x'
        jdk 'JDK 8'
    }

    environment {
        // Define any environment variables needed by your pipeline
        MVN_HOME = tool 'Maven 3.x'
        PATH = "${env.MVN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code from the repository...'
                checkout scm
                echo 'Code checkout completed.'
            }
        }

        stage('Build') {
            steps {
                echo 'Starting the build process...'
                sh 'mvn clean install'
                echo 'Build process completed.'
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
                echo 'Unit tests completed.'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Starting code analysis using SonarQube...'
                sh 'mvn sonar:sonar'
                echo 'Code analysis completed.'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package'
                echo 'Packaging completed.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application to the staging environment...'
                sh 'mvn deploy'
                echo 'Deployment completed.'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up the workspace...'
            cleanWs()
            echo 'Workspace cleanup completed.'
        }

        success {
            echo 'Build was successful!'
        }

        failure {
            echo 'Build failed. Sending email notification...'
            emailext (
                to: 'you@example.com',
                subject: "Build Failed: ${currentBuild.fullDisplayName}",
                body: "Build failed. Please check the Jenkins console output."
            )
            echo 'Email notification sent.'
        }
    }
}
