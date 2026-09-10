pipeline {
    agent any

    environment {
        SERVER = "azureuser@20.127.107.174"
        APP_DIR = "/var/www/dotnetdemo"
    }

    stages {

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build -c Release --no-restore'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release --no-build -o publish'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                rsync -av --delete publish/ ${SERVER}:${APP_DIR}/
                '''
            }
        }

        stage('Restart Application') {
            steps {
                sh '''
                ssh ${SERVER} "sudo systemctl restart dotnetdemo"
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                ssh ${SERVER} "systemctl is-active dotnetdemo"
                curl -f http://20.127.107.174
                '''
            }
        }
    }
}
