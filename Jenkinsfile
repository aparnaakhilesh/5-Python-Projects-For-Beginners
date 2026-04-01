pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install --upgrade pip

                if [ -f requirements.txt ]; then
                    pip install -r requirements.txt
                fi
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
