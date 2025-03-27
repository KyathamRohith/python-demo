pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KyathamRohith/python-demo.git'
            }
        }

       stage('Install Dependencies') {
            steps {
                sh '''
                echo "Current directory: $(pwd)"
                
                # Create and activate a virtual environment
                python3 -m venv venv
                source venv/bin/activate
                
                # Upgrade pip and install dependencies inside venv
                venv/bin/python -m pip install --upgrade pip
                venv/bin/pip install -r requirements.txt
                
                deactivate
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                echo "Running tests..."
                ls -R  # Debugging: List directory structure
                
                # Activate virtual environment and run tests
                source venv/bin/activate
                PYTHONPATH=$(pwd)/python-demo venv/bin/python -m unittest discover -s python-demo/tests -p "test_*.py"
                deactivate
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Building the project..."
                mkdir -p build
                cp -r python-demo/src/* build/
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                sh '''
                echo "Archiving artifacts..."
                mkdir -p build  # Ensure build directory exists
                tar -czf python-demo.tar.gz -C build .
                '''
                archiveArtifacts artifacts: 'python-demo.tar.gz', fingerprint: true
            }
        }
    }
}
