pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Nesa0131/simple-django-app.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                sudo apt update -y
                sudo apt install -y python3-venv python3-pip
                python3 -m venv .venv
                . .venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt || pip install django gunicorn pylint
                '''
            }
        }

        stage('Run Linter (pylint)') {
            steps {
                sh '''
                . .venv/bin/activate
                pylint cool_counters/cool_counters cool_counters/counter || true
                '''
            }
        }

        stage('Run Django Server') {
            steps {
                sh '''
                . .venv/bin/activate
                cd cool_counters
                nohup python3 manage.py runserver 0.0.0.0:8000 &
                sleep 5
                curl -sSf http://localhost:8000/ || echo "La aplicación no se pudo iniciar correctamente"
                '''
            }
        }
    }
}
