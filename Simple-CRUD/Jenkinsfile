pipeline {
    agent any

    environment {
        // Define variáveis de ambiente pra facilitar o uso dos paths
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
        PYTHON = 'python3'
    }

    stages {

        stage('Clonar Repositório') {
            steps {
                // Clona o código do GitHub 
                echo 'Clonando repositório...'
            }
        }

        stage('Instalar dependências do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    // Instala os pacotes do React
                    sh 'npm install'
                }
            }
        }

        stage('Build do frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    // Executa o build do React
                    sh 'npm run build'
                }
            }
        }

        stage('Instalar dependências do backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    // Cria e ativa ambiente virtual, instala pacotes do Flask
                    sh '''
                    ${PYTHON} -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Rodar o backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    // Sobe o Flask 
                    sh '''
                    . venv/bin/activate
                    export FLASK_APP=app.py
                    flask run --host=0.0.0.0 --port=5000 &
                    '''
                }
            }
        }
    }
}
