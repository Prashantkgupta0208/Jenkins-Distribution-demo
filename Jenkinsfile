pipeline{
stage('Deploy Website on Controller') {

    agent {
        label 'controller'
    }

    steps {

        sh '''
        # Display current machine hostname
        hostname

        # Copy website from Jenkins workspace to Apache web root
        sudo cp index.html /var/www/html/index.html

        # Restart Apache service
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
        # Display current machine
        hostname

        # Verify Apache serves the website
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
            # Remove old virtual environment
            rm -rf venv

            # Create virtual environment
            python3 -m venv venv

            # Activate virtual environment
            . venv/bin/activate

            # Install required packages
            pip install -r requirements.txt

            # Stop existing calculator process
            pkill -f app.py || true

            # Start calculator in background
            nohup python app.py > app.log 2>&1 &

            # Wait for startup
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

        # Check if application is running
        ps -ef | grep app.py | grep -v grep || true

        # Check port 5000
        ss -tulpn | grep 5000 || true
        '''
    }
}
}
