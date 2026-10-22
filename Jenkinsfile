pipeline {

    agent none

    stages {

        stage('Deploy Website on Controller') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                hostname

                sudo cp index.html /var/www/html/index.html

                sudo systemctl restart apache2

                echo "Website deployed successfully"
                '''
            }
        }

        stage('Verify Website') {

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

        stage('Deploy Calculator') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    sh '''
                    rm -rf venv

                    python3 -m venv venv

                    . venv/bin/activate

                    pip install -r requirements.txt

                    pkill -f app.py || true

                    nohup python app.py > app.log 2>&1 &

                    sleep 5

                    echo "Calculator started"
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

                ss -tulpn | grep 5000 || true
                '''
            }
        }
    }
}
