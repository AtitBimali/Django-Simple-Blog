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
                sh 'pip3 install -r requirements.txt'
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
                sh 'cp -r * /var/www/html/app/'
                // sh 'systemctl restart gunicorn'
                // sh 'systemctl restart nginx'

                script{
                def projectFolder = '/var/www/html/app/'
                dir(projectFolder) {
                        sh 'gunicorn BlogPetProject.wsgi &'
                    }
                }
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