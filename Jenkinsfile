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
            sh """
                ${SCANNER_HOME}/bin/sonar-scanner \
                -Dsonar.projectKey=python-beginner-projects \
                -Dsonar.projectName="Python Beginner Projects" \
                -Dsonar.sources=. \
                -Dsonar.inclusions=**/*.py \
                -Dsonar.exclusions=**/*.txt,**/*.key,**/*.md,**/__pycache__/** \
                -Dsonar.python.version=3.10
            """
        }
    }
}
 
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
