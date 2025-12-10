pipeline {
    agent any

    // This references the 'LocalMaven' tool we configured in Jenkins
    tools {
        maven 'LocalMaven' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Uses the configuration from Jenkins SCM settings
                checkout scm 
            }
        }

        stage('Maven Build and Install') {
            steps {
                // Updated command to skip tests (since they fail due to database connection issues)
                sh 'mvn clean install -DskipTests' 
            }
        }
    }
}
