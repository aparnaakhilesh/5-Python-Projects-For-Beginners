pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'
        }
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python --version
                python -m venv venv
                source venv/bin/activate
                pip install --upgrade pip
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=python-beginner-projects \
                    -Dsonar.sources=.
                    '''
                }
            }
        }

        stage('Run Project') {
            steps {
                sh '''
                source venv/bin/activate
                echo "Project executed successfully"
                '''
            }
        }
    }
}
