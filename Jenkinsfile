pipeline {

    agent none

    stages {

        stage('Deploy HTML on Master') {

            agent {
                label 'master'
            }

            steps {

                sh '''
                echo "Deploying Website"

                pwd
                ls -lrt

                sudo cp index.html /var/www/html/index.html

                sudo systemctl restart apache2

                echo "Website Deployed Successfully"
                '''
            }
        }

        stage('Verify Master Deployment') {

            agent {
                label 'master'
            }

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
                echo "Running on Worker"

                hostname
                whoami
                pwd
                '''
            }
        }
    }
}
