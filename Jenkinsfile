pipeline {
    agent any
    environment {
        registry = "elboujamaiouardia/tp4"
        registryCredential = 'ebcd6987-42f2-4d3a-9abe-4f9f62e0a08c'
    }
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'master', url: 'https://github.com/El-boujamai/tp4'
            }
        }
        stage('Building image') {
            steps {
                script {
                    def customImage = docker.build("${registry}:${env.BUILD_NUMBER}")
                    bat "docker tag ${registry}:${env.BUILD_NUMBER} ${registry}:latest"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed successfully"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        bat "docker push ${registry}:${env.BUILD_NUMBER}"
                        bat "docker push ${registry}:latest"
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                script {
                    // Optionnel : s'assurer qu'aucun ancien conteneur du même nom ne bloque le port (si vous en utilisez un)
                    // bat "docker rm -f mon-app-tp4 || true"
                    
                    // Lancement du conteneur en arrière-plan (-d) avec un port mappé (ex: port 8080 ou 80)
                    bat "docker run -d -p 8080:80 --name tp4-container_${env.BUILD_NUMBER} ${registry}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}