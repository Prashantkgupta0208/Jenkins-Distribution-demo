pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Prashantkgupta0208/Jenkins-Distribution-demo.git'
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                sudo cp index.html /var/www/html/index.html
                sudo systemctl restart apache2
                '''
            }
        }

    }
}
