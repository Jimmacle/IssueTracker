pipeline {
    agent any
    environment {
        APP_NAME = "issuetracker"
    }
    stages {
        stage('Build') {
            steps {
                script {
                    app = docker.build("${APP_NAME}:${BRANCH_NAME}", "-f ./IssueTracker/Dockerfile")
                }
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                sh "docker tag ${app.id} jimmacle/${APP_NAME}:latest"
                sh "docker stop ${APP_NAME} || true"
                sh "docker rm ${APP_NAME} || true"
                script {
                    app.run("--name ${APP_NAME} --network proxy")
                }
            }
        }
    }
}