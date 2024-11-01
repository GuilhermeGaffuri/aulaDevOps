pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Puxar o código do repositório Git
                git 'https://github.com/GuilhermeGaffuri/aulaDevOps.git'
            }
        }
        
        stage('Build Docker Images') {
            steps {
                // Construir as imagens Docker definidas no docker-compose.yml
                sh 'docker-compose -f docker-compose.yml build'
            }
        }
        
        stage('Run Application') {
            steps {
                // Subir os containers com Docker Compose
                sh 'docker-compose -f docker-compose.yml up -d'
            }
        }

        stage('Tests') {
            steps {
                // Executar testes na aplicação
                sh 'docker-compose -f docker-compose.yml run --rm flask_app pytest'
            }
        }

        stage('Cleanup') {
            steps {
                // Parar e remover containers ao finalizar
                sh 'docker-compose -f docker-compose.yml down'
            }
        }
    }

    post {
        always {
            // Limpar quaisquer containers ou redes criadas durante a pipeline
            sh 'docker-compose -f docker-compose.yml down --volumes --remove-orphans'
        }
        success {
            echo 'A pipeline foi executada com sucesso!'
        }
        failure {
            echo 'A pipeline falhou.'
        }
    }
}
