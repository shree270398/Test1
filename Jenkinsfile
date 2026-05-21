pipeline {
    agent any
    environment {
        IMAGE_NAME = "shree270398/test1"
        CONTAINER_NAME = "test1-container"
    }

    stages {

        stage('Clone GitHub') {
            steps {
                git branch: 'main',
                url: 'https://github.com/shree270398/Test1.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -Dskiptests'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker ') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }


        stage('Run Docker Container') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }

   
}
