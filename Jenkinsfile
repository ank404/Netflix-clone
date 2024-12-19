pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anupk40/netflix-app"
        SONAR_SCANNER = "SonarQubeScanner"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner \
                        -Dsonar.projectKey=Netflix-App \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarqube.anupkhanal.info.np:9000 \
                        -Dsonar.login='
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build --build-arg TMDB_V3_API_KEY=68f4ec849382384d31b163f55edd3c0a -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
