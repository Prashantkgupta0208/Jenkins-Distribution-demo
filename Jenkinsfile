pipeline {

    agent none

    stages {

        stage('Deploy Website on Controller') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== CONTROLLER NODE ====="

                hostname

                # Copy website to Apache web root
                sudo cp index.html /var/www/html/index.html

                # Restart Apache
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
                    echo "===== WORKER NODE ====="

                    hostname

                    # Activate existing virtual environment
                    . venv/bin/activate

                    # Install any new packages
                    pip install -r requirements.txt

                    # Stop old app if running
                    pkill -f app.py || true

                    # Start calculator in background
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

                dir('calculator') {

                    sh '''
                    hostname

                    echo "Running Process"

                    ps -ef | grep app.py | grep -v grep || true

                    echo "Listening Port"

                    ss -tulpn | grep 5000 || true
                    '''
                }
            }
        }
    }
}
