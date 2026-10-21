pipeline {

    agent none

    stages {

        stage('Deploy HTML on Master') {

            agent any

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
    }
}
        
