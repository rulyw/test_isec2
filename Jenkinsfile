pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }

    environment {
        // Replace with your actual DockerHub or registry details
        DOCKER_IMAGE = 'ruly_w/testass2'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = '15a57bf4-72e1-4a6c-9091-f7bbe460d5cb' // Jenkins credentials ID
        SNYK_TOKEN = credentials('fac1903f-ac4d-4155-bf67-bec3960a5375')   // Jenkins secret text credential for Snyk
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install --save'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'npm test'
            }
        }

        stage('Security Scan with Snyk') {
            steps {
                echo 'Running security scan using Snyk...'
                sh '''
                    npm install -g snyk
                    snyk auth ${SNYK_TOKEN}
                    snyk test --severity-threshold=high
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to registry...'
                withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDENTIALS}", passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh '''
                        echo "$DOCKER_PASS" | doc_
