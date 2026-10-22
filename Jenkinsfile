
pipeline {

    agent none

    stages {

        stage('Deploy Website on Master') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== MASTER NODE ====="

                hostname

                pwd

                ls -lrt

                sudo cp index.html /var/www/html/index.html

                sudo systemctl restart apache2

                echo "Website Deployed Successfully"
                '''
            }
        }

        stage('Verify Website') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== VERIFY WEBSITE ====="

                hostname

                curl http://localhost
                '''
            }
        }

        stage('Checkout Calculator Repo') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    git branch: 'main',
                    url: 'https://github.com/Prashantkgupta0208/Python-calculator.git'
                }
            }
        }

        stage('Deploy Calculator on Worker') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    sh '''
                    echo "===== WORKER NODE ====="

                    hostname

                    python3 --version

                    ls -lrt

                    pip3 install -r requirements.txt || true

                    pkill -f app.py || true

                    nohup python3 app.py > app.log 2>&1 &

                    echo "Calculator Started"
                    '''
                }
            }
        }

        stage('Verify Calculator') {

            agent {
                label 'node-1'
            }

            steps {

                sh '''
                hostname

                ps -ef | grep app.py | grep -v grep || true

                ss -tulpn
                '''
            }
        }
    }
}
