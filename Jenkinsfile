pipeline {
    agent any
    tools {
        // This must match the name in Jenkins > Manage Jenkins > Global Tool Configuration
        maven 'LocalMaven' 
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout is implicit when using Pipeline from SCM
                echo 'Checking out source code...'
            }
        }
        
        stage('Build') {
            steps {
                // Compile the code, skip tests (-DskipTests) and package it into a JAR
                sh "mvn clean package -DskipTests"
            }
            post {
                success {
                    // Archive the resulting JAR artifact
                    archiveArtifacts artifacts: 'target/student-management-0.0.1-SNAPSHOT.jar', onlyIfSuccessful: true
                }
            }
        }
        
        // **********************************************
        // ********* NEW DOCKER STAGE ADDED HERE ********
        // **********************************************
        stage('Build Docker Image') {
            steps {
                script {
                    // Define the Docker image name and tag
                    // REPLACE 'your_dockerhub_username' with your actual Docker Hub ID
                    def imageTag = "ajimi0/student-app:${env.BUILD_NUMBER}"
                    
                    // Build the Docker image
                    // -t: tag the image
                    // .: the build context is the root of the project, which contains the 'target' folder and 'docker/Dockerfile'
                    // -f: specify the path to the Dockerfile
                    sh "sudo docker build -t ${imageTag} -f docker/Dockerfile ."
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
