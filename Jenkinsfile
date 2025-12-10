pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ab2sfaxii/avec-maven'
            }
        }

        stage('MAVEN') {
            steps {
                sh 'mvn -version'
            }
        }
    }
}



