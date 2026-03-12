pipeline {

agent any

environment {
    DOCKER_IMAGE = "harshitjaiswal394/todo-application:latest"
}

stages {

    stage('Clone Repository') {
        steps {
            git branch: 'master',
            url: 'https://github.com/harshitjaiswal394/todo-application.git'
        }
    }

    stage('Build Maven Project') {
        steps {
            sh 'mvn clean package -DskipTests'
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'sudo docker build -t $DOCKER_IMAGE .'
        }
    }

    stage('Push Docker Image') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'docker-hub-credentials',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {

                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                sudo docker push $DOCKER_IMAGE
                '''
            }
        }
    }

    stage('Deploy using Docker Compose') {
        steps {
            sh '''
            sudo docker compose down
            sudo docker compose up -d
            '''
        }
    }

    stage('Verify Containers') {
        steps {
            sh 'sudo docker ps'
        }
    }

    stage('Clean Workspace') {
        steps {
            sh 'sudo rm -rf *'
        }
    }
}

}

