pipeline {
    agent any

    stages {
        stage('Build Test Image') {
            steps {
                sh '''
                    docker build -t my-python-app-test .
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    mkdir -p test-results

                    docker run --rm \
                        -v "$WORKSPACE/test-results:/app/test-results" \
                        my-python-app-test \
                        pytest -v --junitxml=/app/test-results/pytest.xml
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/*.xml'
        }
    }
}
