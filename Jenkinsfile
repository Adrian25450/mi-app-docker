pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'registry.hub.docker.com'
        DOCKER_IMAGE = 'adrian0526/mi-app-docker'
        DOCKER_CREDS = credentials('docker-hub-credentials')
        BUILD_VERSION = "1.0.${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Adrian25450/mi-app-docker.git', branch: 'main'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}", "--build-arg BUILD_VERSION=${BUILD_VERSION} .")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def testImage = docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}")
                    testImage.inside {
                        sh '''
                            echo "Verificando que PHP esté activo..."
                            php -v
                            curl -s http://localhost || echo "No se pudo acceder al servidor"
                        '''
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push()
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push('latest')
                    }
                }
            }
        }

        stage('Deploy to Test') {
            steps {
                script {
                    sh """
                        docker stop test-app || true
                        docker rm test-app || true
                        docker run -d -p 8081:80 --name test-app ${DOCKER_IMAGE}:${env.BUILD_ID}
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline ${currentBuild.result ?: 'SUCCESS'}"
            archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
        }
        success {
            echo '¡Despliegue exitoso!'
        }
        failure {
            echo 'Despliegue fallido'
        }
    }
}
