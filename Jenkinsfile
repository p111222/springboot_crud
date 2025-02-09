pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "springboot-crud-app"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/p111222/springboot_crud.git'
            }
        }
        stage('Build JAR') {
            steps {
                bat '''
                cd Springboot/test1
                mvn clean install -DskipTests
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                bat '''
                cd Springboot/test1
                docker build -t %DOCKER_IMAGE% -f Dockerfile .
                '''
            }
        }
        stage('Run Container') {
            steps {
                bat '''
                docker stop springboot-crud-app || exit 0
                docker rm springboot-crud-app || exit 0
                docker run -d --name springboot-crud-app -p 8081:8080 %DOCKER_IMAGE%
                '''
            }
        }
    }
}
