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
                // Executes the build using the tool path set by 'tools'
                sh 'mvn clean install'
            }
        }
    }
}
