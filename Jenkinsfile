pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer'
        DOCKER_IMAGE = 'myapp:latest'
        NVD_API_CRED = 'nvd-api-key'    // Jenkins credentials ID for NVD API key
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/banita1985/Sample-javawebapp.git' // Replace with your actual GitHub repo
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests -Ddependency-check.skip=true'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }


        stage('Docker Build') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Image Scan with Trivy') {
            steps {
                sh 'trivy image ${DOCKER_IMAGE} || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8085:8080 ${DOCKER_IMAGE}'
            }
        }
    }
}
