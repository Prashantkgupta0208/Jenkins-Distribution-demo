pipeline {

    agent none

    stages {

        stage('Checkout Controller Website') {

            agent {
                label 'controller'
            }

            steps {

                git branch: 'main',
                url: 'https://github.com/Prashantkgupta0208/Jenkins-Distribution-demo.git'
            }
        }

        stage('Deploy Controller Website') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== CONTROLLER NODE ====="

                hostname

                sudo cp index.html /var/www/html/index.html

                sudo systemctl restart apache2

                echo "Controller Website Deployed"
                '''
            }
        }

        stage('Verify Controller Website') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                hostname

                curl http://localhost
                '''
            }
        }

        stage('Checkout Worker Website') {

            agent {
                label 'node-1'
            }

            steps {

                dir('worker-site') {

                    git branch: 'main',
                    url: 'https://github.com/Prashantkgupta0208/html-code-Node-1-Jenkins.git'
                }
            }
        }

        stage('Deploy Worker Website') {

            agent {
                label 'node-1'
            }

            steps {

                dir('worker-site') {

                    sh '''
                    echo "===== WORKER NODE ====="

                    hostname

                    sudo cp index.html /var/www/html/index.html

                    sudo systemctl restart apache2

                    echo "Worker Website Deployed"
                    '''
                }
            }
        }

        stage('Verify Worker Website') {

            agent {
                label 'node-1'
            }

            steps {

                sh '''
                hostname

                curl http://localhost
                '''
            }
        }
    }
}
