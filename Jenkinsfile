pipeline {
agent any

```
environment {
    DOCKER_USER = credentials('dockerhub-user')
    DOCKER_PASS = credentials('dockerhub-password')
    EMAIL_DESTINO = credentials('email-destino')
}

stages {

    stage('Instalar Dependências') {
        steps {
            dir('backend') {
                sh 'python -m pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
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

    stage('Build Docker') {
        steps {
            sh 'docker build -t SEU_USUARIO_DOCKERHUB/task-manager:latest ./backend'
        }
    }

    stage('Push Docker Hub') {
        steps {
            sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push SEU_USUARIO_DOCKERHUB/task-manager:latest
            '''
        }
    }

    stage('Enviar Email') {
        steps {
            sh 'python backend/send_email.py'
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
```

}
