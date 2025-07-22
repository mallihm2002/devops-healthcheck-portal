pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/mallihm2002/devops-healthcheck-portal.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'pip install -r requirements.txt'
                    sh 'python app.py & sleep 5' // Simulate running app
                    sh 'curl http://localhost:5000/health'
                }
            }
        }

        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh '''
                    response=$(curl -s http://localhost:5000/health)
                    if [[ "$response" != *"status"* ]]; then
                        echo "Health check failed!"
                        exit 1
                    fi
                    '''
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.py', fingerprint: true
            }
        }
    }
}
