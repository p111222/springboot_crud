pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "my-dockerhub-username/springboot-crud-app"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/p111222/springboot_crud.git'
            }
        }
        stage('Build JAR') {
            steps {
                bat 'cd Springboot/test1 & mvn clean install -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %DOCKER_IMAGE% .
                docker tag %DOCKER_IMAGE%:latest %DOCKER_IMAGE%:1.0
                '''
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/']) {
                    bat '''
                    docker push %DOCKER_IMAGE%:latest
                    docker push %DOCKER_IMAGE%:1.0
                    '''
                }
            }
        }
        stage('Run Container') {
            steps {
                bat '''
                docker stop springboot-crud-app || exit 0
                docker rm springboot-crud-app || exit 0
                docker run -d --name springboot-crud-app -p 8080:8080 %DOCKER_IMAGE%:latest
                '''
            }
        }
    }
}
