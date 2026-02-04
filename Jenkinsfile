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
                   python3 --version
                   pip3 install --upgrade pip
                   pip3 install -r requirements.txt
                '''
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                   pytest --junitxml=results.xml
                '''
            }
        }
    }

    post {
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        always {
            junit 'results.xml'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
