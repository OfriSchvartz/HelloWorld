    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'ofris99/helloworld'
    }
    stages {
        stage('Code Checkout'){
            steps{
                checkout scm
            }
        }
        stage('Docker Build + Tag'){
            steps{
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                    sh 'docker tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }
        stage('Docker Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                    usernameVariable:'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push ${IMAGE_NAME}:latest
                docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
                }
            }
        }
    }
