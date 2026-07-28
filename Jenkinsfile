pipeline {
    agent any

    environment {
        IMAGE_NAME = "spring-petclinic:v1"
        CONTAINER_NAME = "petclinic"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true

                docker run -d --name $CONTAINER_NAME -p 8081:8080 $IMAGE_NAME
                '''
            }
        }
    }
}
