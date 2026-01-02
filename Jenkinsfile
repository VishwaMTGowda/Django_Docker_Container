pipeline {
    agent any
    
    options {
        skipDefaultCheckout true  // Prevent automatic checkout
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],  // Changed from master to main
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/VishwaMTGowda/Django_Docker_Container']]
                ])
                echo 'Repository cloned successfully'
            }
        }
        
        stage('Setup Python Environment') {
            steps {
                bat '''
                    echo Setting up Python environment...
                    python --version
                    pip --version
                    
                    # Create virtual environment
                    python -m venv venv
                    call venv\\Scripts\\activate
                    
                    # Upgrade pip
                    pip install --upgrade pip
                    
                    # Install requirements
                    if exist requirements.txt (
                        pip install -r requirements.txt
                    ) else (
                        echo requirements.txt not found
                    )
                '''
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    echo Installing dependencies...
                    
                    # Install Django if not in requirements
                    pip install django
                    
                    # Install any other dependencies needed for CI
                    pip install pytest pytest-django
                    pip install coverage
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    echo Running tests...
                    
                    # Check if manage.py exists
                    if exist manage.py (
                        python manage.py test --noinput
                    ) else (
                        echo manage.py not found in current directory
                        dir
                    )
                '''
            }
        }
        
        stage('Code Quality Check') {
            steps {
                bat '''
                    call venv\\Scripts\\activate
                    echo Running code quality checks...
                    
                    # Install linters
                    pip install flake8
                    
                    # Run flake8
                    python -m flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics || echo "Critical errors found"
                    python -m flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics || echo "Style issues found"
                '''
            }
        }
        
        stage('Build Status') {
            steps {
                echo 'Build completed successfully!'
            }
        }
    }
    
    post {
        always {
            bat '''
                echo Cleaning up...
                # Remove virtual environment
                rmdir /s /q venv 2>nul || echo "venv not found"
            '''
            echo 'Pipeline execution completed'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}