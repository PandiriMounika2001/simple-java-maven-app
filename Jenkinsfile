pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'httpd'
        DOCKER_REGISTRY = 'mounika2001/httpd' // Docker Hub repository path
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Deploy to DEV') {
            when {
                branch 'master'
            }
            environment {
                ENVIRONMENT = 'dev'
            }
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        dockerImage.push("${DOCKER_IMAGE}:latest")
                    }
                    sh "kubectl apply -f ./k8s/${env.ENVIRONMENT}.yaml"
                }
            }
        }
        
        stage('Deploy to UAT') {
            when {
                branch 'release/*'
            }
            environment {
                ENVIRONMENT = 'uat'
            }
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        dockerImage.push("${DOCKER_IMAGE}:latest")
                    }
                    sh "kubectl apply -f ./k8s/${env.ENVIRONMENT}.yaml"
                }
            }
        }
        
        stage('Deploy to PROD') {
            when {
                branch 'main'
            }
            environment {
                ENVIRONMENT = 'prod'
            }
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        dockerImage.push("${DOCKER_IMAGE}:latest")
                    }
                    sh "kubectl apply -f ./k8s/${env.ENVIRONMENT}.yaml"
                }
            }
        }
    }
    
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}









