pipeline {

    agent none

    stages {

        /*
        ==========================================
        CONTROLLER NODE STAGES
        ==========================================
        */

        stage('Deploy Website on Controller') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== CONTROLLER NODE ====="

                # Show machine name
                hostname

                # Deploy website to Apache location
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
                echo "===== VERIFY WEBSITE ====="

                hostname

                # Verify website content
                curl http://localhost
                '''
            }
        }

        /*
        ==========================================
        WORKER NODE STAGES
        ==========================================
        */

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

        stage('Prepare Python Environment') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    sh '''
                    echo "===== PREPARE PYTHON ENV ====="

                    hostname

                    # Create virtual environment only first time
                    if [ ! -d "venv" ]; then
                        python3 -m venv venv
                    fi

                    echo "Virtual Environment Ready"
                    '''
                }
            }
        }

        stage('Install Dependencies') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    sh '''
                    # Activate venv
                    . venv/bin/activate

                    # Install packages from requirements.txt
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Start Calculator') {

            agent {
                label 'node-1'
            }

            steps {

                dir('calculator') {

                    sh '''
                    # Activate venv
                    . venv/bin/activate

                    # Stop old process if running
                    pkill -f app.py || true

                    # Start calculator in background
                    nohup python app.py > app.log 2>&1 &

                    sleep 5

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

                dir('calculator') {

                    sh '''
                    echo "===== VERIFY CALCULATOR ====="

                    hostname

                    # Check process
                    ps -ef | grep app.py | grep -v grep || true

                    # Check listening port
                    ss -tulpn | grep 5000 || true
                    '''
                }
            }
        }
    }
}
