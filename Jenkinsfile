pipeline {
    agent any
    
    stages {
        stage("Build") {
            steps {
                echo "Building ..."
                // Add your build steps here
            }
            post {
                always {
                    mail to: "gurdarshan24k@gmail.com",
                         subject: "Build Status Email",
                         body: "Build log attached!"
                }
            }
        }
        
        stage("Test") {
            steps {
                echo "Testing ..."
                // Add your test steps here
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying ..."
                // Add your deploy steps here
            }
        }
    }
}
