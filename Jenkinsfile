pipeline {
    agent any

    environment {
        MAVEN_TOOL   = 'maven3.9.12'

        DOCKER_IMAGE = 'code9bruno/linkpay'
        DOCKER_TAG   = "${BUILD_NUMBER}"

        TEST_CONTAINER = 'linkpay-test'
        PROD_CONTAINER = 'linkpay-prod'
    }

    stages {

        stage('1. Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/code9Bruno/pipeline1.git'
            }
        }

        stage('2. Maven Build') {
            steps {
                withMaven(maven: "${MAVEN_TOOL}") {
                    sh 'mvn clean package -Dmaven.test.skip=true'
                }
            }
        }

        stage('3. Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh """
                    docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                """
            }
        }

        stage('4. Push Docker Image to DockerHub') {
            steps {
                echo '📤 Pushing Docker image to Docker Hub...'
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-cred',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh """
                        docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
        always {
            echo '🧹 Pipeline finished.'
        }
    }
}
