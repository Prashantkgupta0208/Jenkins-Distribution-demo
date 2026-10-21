pipeline {

    agent none

    stages {

        stage('Checkout Source') {
            agent any

            steps {
                git 'https://github.com/Prashantkgupta0208/Jenkins-Distribution-demo.git'
            }
        }

        stage('Deploy HTML on Master') {
            agent any

            steps {

                sh '''
                echo "Deploying Website"

                sudo cp index.html /var/www/html/index.html

                sudo systemctl restart apache2

                echo "Website Deployed Successfully"
                '''
            }
        }

        stage('Verify Master Deployment') {
            agent any

            steps {
                sh '''
                hostname
                ls -l /var/www/html
                '''
            }
        }

        stage('Run Job on Worker') {

            agent {
                label 'node1'
            }

            steps {

                sh '''
                echo "Executing on Worker Node"

                hostname

                whoami

                pwd
                '''
            }
        }
    }
}
