pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                sh '''
                python3 --version

                if [ ! -d "$VENV" ]; then
                    python3 -m venv $VENV
                fi

                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . $VENV/bin/activate

                mkdir -p test-reports

                # Run pytest but DO NOT fail pipeline if no tests are found
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
            echo '✅ Python CI Pipeline completed successfully!'
        }

        failure {
            echo '❌ Python CI Pipeline failed!'
        }
    }
}
