pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/VishwaMTGowda/Django_Docker_Container'
            }
        }

        stage('Setup Virtualenv') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Migrations') {
            steps {
                sh '''
                . venv/bin/activate
                python mysite/manage.py migrate
                '''
            }
        }

        stage('Run Server') {
            steps {
                sh '''
                . venv/bin/activate
                nohup python mysite/manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }
}
