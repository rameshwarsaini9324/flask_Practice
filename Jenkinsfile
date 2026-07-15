pipeline {

    agent any

   environment {
    MONGO_URI = credentials('MONGO_URI')
}

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/rameshwarsaini9324/flask_Practice.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                pkill -f app.py || true
                nohup venv/bin/python app.py > app.log 2>&1 &
                '''
            }
        }
    }

    post {

        success {
            emailext(
                subject: "SUCCESS: ${env.JOB_NAME}",
                body: "Build Successful",
                to: "rameshwarsaini9324@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "FAILED: ${env.JOB_NAME}",
                body: "Build Failed",
                to: "rameshwarsaini9324@gmail.com"
            )
        }
    }
}
