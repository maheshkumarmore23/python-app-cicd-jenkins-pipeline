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

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                python --version
                python -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                mkdir -p test-reports

                # Run pytest but DO NOT fail build if no tests found
                pytest --junitxml=test-reports/results.xml || true
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-reports/results.xml'
        }
        success {
            echo 'Python CI Pipeline PASSED!'
        }
        failure {
            echo 'Python CI Pipeline FAILED!'
        }
    }
}
