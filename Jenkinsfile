pipeline {
    agent {
        docker {
            image 'python:3.11'
            args '-u root'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python --version
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    pytest --junitxml=test-reports/results.xml
                '''
            }
        }
    }

    post {
        always {
            junit 'test-reports/*.xml'
        }
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        failure {
            echo 'Python CI Pipeline failed!'
        }
    }
}
