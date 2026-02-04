pipeline {
    agent {
        docker {
            image 'python:3.11'
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
                    pip --version
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    pip install pytest pytest-html
                '''
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'python -m py_compile app.py'
            }
        }

        stage('Unit Testing') {
            steps {
                sh '''
                    mkdir -p test-reports
                    pytest --junitxml=test-reports/results.xml \
                           --html=test-reports/report.html \
                           --self-contained-html
                '''
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'test-reports',
                        reportFiles: 'report.html',
                        reportName: 'PyTest HTML Report'
                    ])
                }
            }
        }
    }

    post {
        success {
            echo 'Python CI pipeline completed successfully'
        }
        failure {
            echo 'Python CI pipeline failed'
        }
    }
}
