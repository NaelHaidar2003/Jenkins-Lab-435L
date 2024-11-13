pipeline {
    agent any

    environment {
        VIRTUAL_ENV = 'venv'
        PYTHONPATH = "${env.WORKSPACE}"
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && set PYTHONPATH=${env.WORKSPACE} && pytest tests/ --junitxml=report.xml"
                }
            }
        }

        stage('Coverage') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && set PYTHONPATH=${env.WORKSPACE} && coverage run -m pytest && coverage report && coverage xml"
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && bandit -r app.py -f xml -o bandit_report.xml"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && echo 'Deployment logic goes here'"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
