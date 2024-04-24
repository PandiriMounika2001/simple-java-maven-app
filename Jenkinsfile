pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-amd64"
        MAVEN_HOME = "/usr/share/maven"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                script {
                    git credentialsId: '2239466d-ce75-41c1-ab88-537d24881ecc', url: 'https://github.com/PandiriMounika2001/simple-java-maven-app.git'
                }
            }
        }

        stage('Choose Branch') {
            steps {
                script {
                    def branches = sh(script: 'git ls-remote --heads origin', returnStdout: true).trim().readLines().collect { it.replaceAll('^.*\\s+refs/heads/', '') }
                    env.BRANCH_NAME = input message: 'Please choose the branch to build',
                                            ok: 'Validate!',
                                            parameters: [choice(name: 'Branch', choices: branches, description: 'Branch to build')]
                }
            }
        }

        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "refs/remotes/origin/${env.BRANCH_NAME}"]],
                    userRemoteConfigs: [[
                        url: 'https://github.com/PandiriMounika2001/simple-java-maven-app.git',
                        credentialsId: '2239466d-ce75-41c1-ab88-537d24881ecc'
                    ]]
                ])
            }
        }

        stage('Build and Test') {
            steps {
                echo "Building and testing the project..."
                sh "${MAVEN_HOME}/bin/mvn clean install"
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the project..."
                sh "${MAVEN_HOME}/bin/mvn deploy"
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/**/*.xml'
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}

