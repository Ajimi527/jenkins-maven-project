pipeline {
    agent any

    tools {
        maven 'LocalMaven' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm 
            }
        }

        stage('Maven Build and Install') {
            steps {
                // Skips tests, compiles, and packages the application
                sh 'mvn clean install -DskipTests' 
            }
        }
        
        // NEW DEPLOYMENT STAGE
        stage('Simulate Deployment') {
            steps {
                echo 'Simulating deployment of the application artifact...'
                // Assumes your artifact is built to 'target/student-management-0.0.1-SNAPSHOT.jar'
                // This command simply verifies the artifact exists after the build
                sh 'ls -l target/student-management-0.0.1-SNAPSHOT.jar'
                
                // You could add commands here to copy the artifact to a deployment directory
                // or use SSH commands (like 'sshpass' or 'scp') for a real deployment.
                echo 'Deployment simulation successful.'
            }
        }
    }
}
