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
                echo 'Installing Python dependencies...'
                sh '''
                    set -e
                    python3 --version
                    pip3 --version

                    # Install project requirements (PEP 668 safe override)
                    pip3 install --break-system-packages -r requirements.txt

                    # Ensure pytest is available even if not in requirements
                    pip3 install --break-system-packages pytest
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                    set -e
                    mkdir -p test-reports

                    # Run pytest via Python module (avoids PATH issue)
                    python3 -m pytest -q --junitxml=test-reports/results.xml
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
