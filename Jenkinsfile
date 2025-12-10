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
        
        // NEW ARTIFACT ARCHIVING STAGE
        stage('Archive Artifact') {
            steps {
                echo 'Archiving the generated JAR file...'
                // Archives the JAR file created in the target directory
                // Filename structure: target/artifactID-version.jar
                archiveArtifacts artifacts: 'target/student-management-0.0.1-SNAPSHOT.jar', 
                                   fingerprint: true
                echo 'Artifact successfully archived.'
            }
        }
    }
}
