pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Fetch Data') {
            steps {
                sh 'git pull origin main'
            }
        }
        stage('Train Model') {
            steps {
                sh 'pip3 install fastapi uvicorn scikit-learn pandas numpy --break-system-packages'
                sh 'python3 train.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ml-pipeline-app .'
            }
        }
        stage('Deploy Container') {
            steps {
                sh 'docker stop ml-api || true'
                sh 'docker rm ml-api || true'
                sh 'docker run -d -p 8000:8000 --name ml-api ml-pipeline-app'
            }
        }
    }
}
