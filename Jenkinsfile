pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Fetch Data') {
            steps {
                sh 'cd /home/ubuntu/lab-midter-sp26-configs && git pull origin main'
            }
        }
        stage('Train Model') {
            steps {
                sh 'cd /home/ubuntu/lab-midter-sp26-configs && python3 train.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'cd /home/ubuntu/lab-midter-sp26-configs && docker build -t ml-pipeline-app .'
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
