pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub')
        DOCKER_IMAGE = 'ezequielg87/nestjs-pin1:latest'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ezequielg87/mundose-pin1.git'
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        stage('Docker Scan') {
            steps {
                script {
                    sh "docker scout cves ${DOCKER_IMAGE}"
                }
            }
        }
    }
    post {
        always {
            script {
                node {
                    cleanWs()
                }
            }
        }
    }
}