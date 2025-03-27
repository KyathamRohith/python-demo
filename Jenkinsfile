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
                python3 -m pip install --upgrade pip
                python3 -m pip install -r python-demo/requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                echo "Running tests..."
                ls -R  # List directory structure for debugging
                
                # Run tests without requiring __init__.py
                PYTHONPATH=$(pwd)/python-demo python3 -m unittest discover -s python-demo/tests -p "test_*.py"
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
                tar -czf python-demo.tar.gz -C build .
                '''
                archiveArtifacts artifacts: 'python-demo.tar.gz', fingerprint: true
            }
        }
    }
}
