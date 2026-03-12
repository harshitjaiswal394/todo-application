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
            sh 'docker build -t $DOCKER_IMAGE .'
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
                docker push $DOCKER_IMAGE
                '''
            }
        }
    }

    stage('Deploy using Docker Compose') {
        steps {
            sh '''
            docker compose down
            docker compose up -d
            '''
        }
    }

    stage('Verify Containers') {
        steps {
            sh 'docker ps'
        }
    }

    stage('Clean Workspace') {
        steps {
            sh 'rm -rf *'
        }
    }
}
```

}

