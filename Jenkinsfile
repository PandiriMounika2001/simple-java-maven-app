pipeline {
    agent any
    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/PandiriMounika2001/simple-java-maven-app.git']]])
            }
        }
        stage('Choose Branch') {
            steps {
                script {
                    branches = bat(returnStdout: true, script: 'git ls-remote --heads origin').trim().split("\\r?\\n")
                    branchNames = branches.collect { it.split("\\s+")[1].replaceAll("refs/heads/", "") }
                    echo "Available branches: ${branchNames}"
                }
            }
        }
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }
        stage('Build and Test') {
            steps {
                echo 'Building and Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deployment steps here
            }
        }
    }
}







