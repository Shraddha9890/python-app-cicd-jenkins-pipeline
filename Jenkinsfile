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
                echo 'Installing Python dependencies (system pip override)...'
                sh '''
                    python3 --version
                    pip3 --version
                    pip3 install --break-system-packages --upgrade pip
                    pip3 install --break-system-packages -r requirements.txt
                    pip3 install --break-system-packages pytest
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                    mkdir -p test-reports
                    pytest --junitxml=test-reports/results.xml
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-reports/results.xml'
        }
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
