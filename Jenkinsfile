pipeline {
    agent any

    stages {

        stage('Instalar Dependências') {
            steps {
                dir('backend') {
                    sh 'python3 -m pip install --upgrade pip'
                    sh 'pip3 install -r requirements.txt'
                }
            }
        }

        stage('Executar Testes') {
            steps {
                dir('backend') {
                    sh 'pytest -v'
                }
            }
        }

        stage('Gerar Cobertura') {
            steps {
                dir('backend') {
                    sh 'pytest --cov=. --cov-report=html --cov-report=xml'
                }
            }
        }
    }

    post {

        always {
            archiveArtifacts artifacts: 'backend/coverage.xml', allowEmptyArchive: true
            archiveArtifacts artifacts: 'backend/htmlcov/**', allowEmptyArchive: true
        }

        success {
            echo 'Pipeline executado com sucesso!'
        }

        failure {
            echo 'Pipeline falhou!'
        }
    }
}
