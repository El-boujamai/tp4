pipeline {
    agent any
    environment {
        registry = "elboujamaiouardia/tp4"
        registryCredential = 'ebcd6987-42f2-4d3a-9abe-4f9f62e0a08c'
    }
    stages {
        stage('Cloning Git') {
            steps {
                // Utilise votre dépôt GitHub personnel
                git branch: 'master', url: 'https://github.com/El-boujamai/tp4'
            }
        }
        stage('Building image') {
            steps {
                script {
                    // Utilisation de 'def' pour éviter les avertissements Groovy et construction de l'image
                    def customImage = docker.build("${registry}:${env.BUILD_NUMBER}")
                    bat "docker tag ${registry}:${env.BUILD_NUMBER} ${registry}:latest"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    // Étape de test (simulation demandée par le TP)
                    echo "Tests passed successfully"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    // Connexion et envoi sur Docker Hub
                    docker.withRegistry('', registryCredential) {
                        bat "docker push ${registry}:${env.BUILD_NUMBER}"
                        bat "docker push ${registry}:latest"
                    }
                }
            }
        }
    }
}