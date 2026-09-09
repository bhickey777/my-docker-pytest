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

                    echo "Files after pytest:"
                    ls -la test-results
                '''
            }
        }
    }

    post {
        always {
            always {
            sh '''
                echo "Checking test report:"
                pwd
                find . -name "*.xml" -print
            '''
            junit 'test-results/*.xml'
        }
    }
}
