pipeline {
    agent any

    environment {
        // Define environment-specific variables
        GIT_URL = 'https://github.com/PandiriMounika2001/simple-java-maven-app'
        DEV_BRANCH = 'develop'
        UAT_BRANCH = 'uat'
        PROD_BRANCH = 'master'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the Git repository
                checkout([$class: 'GitSCM', branches: [[name: "${DEV_BRANCH}"]], userRemoteConfigs: [[url: "${GIT_URL}"]]])
            }
        }

        stage('Build') {
            steps {
                // Build your code here
                sh 'mvn clean install'
            }
        }

        stage('Deploy to Dev') {
            when {
                branch "${DEV_BRANCH}"
            }
            steps {
                // Deploy to Dev environment
                sh 'echo "Deploying to Dev environment"'
            }
        }

        stage('Deploy to UAT') {
            when {
                branch "${UAT_BRANCH}"
            }
            steps {
                // Deploy to UAT environment
                sh 'echo "Deploying to UAT environment"'
            }
        }

        stage('Deploy to Prod') {
            when {
                branch "${PROD_BRANCH}"
            }
            steps {
                // Deploy to Prod environment
                sh 'echo "Deploying to prod environment"'
            }
        }
    }
}
