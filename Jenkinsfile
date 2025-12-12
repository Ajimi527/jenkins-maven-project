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
                    def imageTag = "ajimi0/student-app:${env.BUILD_NUMBER}"
                    
                    // Build the Docker image
                    sh "docker build -t ${imageTag} -f Dockerfile ."
                }
            }
        }
        
        // **********************************************
        // ********* NEW DOCKER LOGIN & PUSH STAGE *******
        // **********************************************
        stage('Push to Docker Hub') {
            steps {
                // Use the 'dockerhub' credential ID you configured in Jenkins
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    script {
                        def imageTag = "ajimi0/student-app:${env.BUILD_NUMBER}"

                        // 1. Docker Login: Use the variables defined by withCredentials
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin"
                        
                        // 2. Docker Push: Push the newly built image
                        sh "docker push ${imageTag}"
                        
                        // 3. Optional: Logout after pushing
                        sh "docker logout"
                    }
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
