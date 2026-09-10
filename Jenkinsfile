pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/dotnetdemo"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/prembop/DotNetDemo.git'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build -c Release'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o publish'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                rm -rf ${APP_DIR}/*
                cp -r publish/* ${APP_DIR}/
                '''
            }
        }

        stage('Restart Service') {
            steps {
                sh 'sudo systemctl restart dotnetdemo'
            }
        }

    }
}
