pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Database Migration') {
            steps {
                sh 'python3 manage.py migrate'
            }
        }

        stage('Collect Static Files') {
            steps {
                sh 'python3 manage.py collectstatic --noinput'
            }
        }

        stage('Update Code on Azure VM') {
            steps {
                sh 'sudo cp -r * /var/www/html/app/'
                sh 'sudo systemctl restart gunicorn'
                sh 'sudo systemctl restart nginx'
                sh 'sudo gunicorn BlogPetProject.wsgi:application'
            }
    }
    }

    post {
        always {
            script {
                echo "Jenkins pipeline executed successfully!"
            }
        }
}
}