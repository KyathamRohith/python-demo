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

                # Ensure python3-venv and python3-pip are installed
                sudo apt update && sudo apt install -y python3-venv python3-pip

                # Create and activate a virtual environment
                python3 -m venv venv
                source venv/bin/activate

                # Install dependencies inside virtual environment
                venv/bin/pip install --upgrade pip
                venv/bin/pip install -r python-demo/requirements.txt

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
