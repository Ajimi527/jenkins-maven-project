pipeline {
    agent any
    tools {
        maven 'LocalMaven' 
    }
    
    stages {
        // ... (Stages Checkout, Build, Build Docker Image restent inchangés) ...
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    script {
                        def imageTag = "ajimi0/student-app:${env.BUILD_NUMBER}"
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker build ${imageTag}"
                        sh "docker push ${imageTag}"
                        sh "docker logout"
                    }
                }
            }
        }
        
        // **************************************************
        // ********* NOUVELLE ÉTAPE : DEPLOIEMENT K8S *******
        // **************************************************
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def imageTag = "ajimi0/student-app:${env.BUILD_NUMBER}"
                    def kubeConfigPath = "/home/vagrant/.kube/config"

                    // 1. Remplacer le tag dans le YAML
                    // Nous allons utiliser 'sed' pour injecter le nouveau tag
                    // dans le fichier spring-deployment.yaml avant de l'appliquer.
                    // Le répertoire de travail de Jenkins est /var/lib/jenkins/workspace/pipeline-maven
                    // Le fichier est accessible via le chemin partagé /vagrant/kubernetes
                    sh "sed -i 's|ajimi0/student-app:.*|ajimi0/student-app:${env.BUILD_NUMBER}|' /vagrant/kubernetes/spring-deployment.yaml"
                    
                    // 2. Déployer l'application
                    // On définit la variable d'environnement KUBECONFIG avant d'exécuter kubectl
                    // Cela indique à kubectl où trouver le fichier de configuration du cluster.
                    sh "KUBECONFIG=${kubeConfigPath} kubectl apply -f /vagrant/kubernetes/spring-deployment.yaml"
                    
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
