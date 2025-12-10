pipeli    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_creds')
        IMAGE_NAME = "abderahmene/avec-maven:1.0.0"
    }

    stages {
        stage('GIT') {
           pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_creds')
        IMAGE_NAME = "abderahmene/appspringfoyer"
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ab2sfaxii/avec-maven'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                docker push ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s/mysql-deployment.yaml -n devops
                kubectl apply -f k8s/spring-deployment.yaml -n devops
                kubectl get pods -n devops
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline terminé avec succès : Build + Docker + Push + Déploiement Kubernetes ✅'
        }
        failure {
            echo '❌ Erreur dans le pipeline (build, push ou déploiement)'
        }
    }
}

