pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies in venv...'
                sh '''
                    python3 --version
                    python3 -m venv venv
                    . venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                    pip install pytest
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                    . venv/bin/activate
                    mkdir -p test-reports
                    pytest --junitxml=test-reports/results.xml
                '''
            }
        }
    }

    post {
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        always {
            junit allowEmptyResults: true, testResults: 'test-reports/results.xml'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
