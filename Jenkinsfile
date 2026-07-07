pipeline {
    agent any

    environment {
        IMAGE_NAME = "task-tracker-app"
        COMPOSE_FILE = "docker-compose.yml"
    }

    stages {

        stage('SCM Pull') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies and Run Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose -f $COMPOSE_FILE up -d --build'
            }
        }

        stage('Curl') {
            steps {
                sh 'curl http://localhost:3000/'
                sh 'curl http://localhost:3000/health'
                sh 'curl http://localhost:3000/api/tasks'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker-compose -f $COMPOSE_FILE down'
                sh 'docker image prune -f'
                cleanWs()
            }
        }
    }
}
