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
                    branches = sh(returnStdout: true, script: 'git ls-remote --heads origin').trim().split("\\r?\\n")
                    branchNames = branches.collect { it.split("\\s+")[1].replaceAll("refs/heads/", "") }
                    echo "Available branches: ${branchNames}"
                }
            }
        }
        stage('Checkout') {
            steps {
                script {
                    branchName = input(message: 'Which branch do you want to build?', parameters: [choice(choices: branchNames, description: 'Select branch to build')])
                    checkout([$class: 'GitSCM', branches: [[name: "*/${branchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/PandiriMounika2001/simple-java-maven-app.git']]])
                }
            }
        }
        stage('Build and Test') {
            steps {
                bat 'mvn -B -DskipTests clean install'
            }
        }
        stage('Deploy') {
            steps {
                bat 'mvn -B clean deploy'
            }
        }
    }
}


