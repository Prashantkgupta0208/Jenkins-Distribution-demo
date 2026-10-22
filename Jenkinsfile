pipeline {

    agent none

    stages {

        /*
        ==========================================
        CONTROLLER NODE
        WEBSITE DEPLOYMENT
        ==========================================
        */

        stage('Deploy Website on Controller') {

            agent {
                label 'controller'
            }

            steps {

                sh '''
                echo "===== CONTROLLER NODE ====="

                # Display current machine
                hostname

                # Copy website to Apache web folder
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

                curl http://localhost
                '''
            }
        }

        /*
        ==========================================
        WORKER NODE
        PYTHON CALCULATOR
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

                    # Create venv ONLY if it does not exist
                    if [ ! -f "venv/bin/activate" ]; then

                        echo "Creating Python Virtual Environment"

                        rm -rf venv

                        python3 -m venv venv

                    fi

                    ls -la venv/bin/activate

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
                    . venv/bin/activate

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
                    . venv/bin/activate

                    # Stop old instance
                    pkill -f app.py || true

                    # Start calculator
                    nohup python app.py > app.log 2>&1 &

              
