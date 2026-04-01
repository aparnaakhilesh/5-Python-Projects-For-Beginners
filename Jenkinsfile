pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/aparnaakhilesh/5-Python-Projects-For-Beginners.git'
            }
        }

        stage('Validate Python') {
            steps {
                sh '''
                python3 --version
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
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
