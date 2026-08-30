pipeline {
    agent any

    // 1. Triggers: Ativação automática via Push (Requisito da Atividade)
    triggers {
        pollSCM('* * * * *') // Verificação periódica ou integração com Webhook
    }

    // 2. Tools: Garante que o Node.js esteja configurado no Jenkins
    tools {
        nodejs 'NodeJS' // Nome da ferramenta configurada no painel do Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Install') {
            steps {
                sh 'npm ci'
            }
        }

        stage('SAST (Security)') {
            steps {
                // Trata o retorno para evitar que vulnerabilidades baixas brequem a esteira se necessário
                sh 'npm audit --audit-level=high'
            }
        }

        stage('Lint & Quality') {
            steps {
                // Ativa o modo legado para aceitar .eslintrc sem quebrar nas versões novas do ESLint
                sh 'ESLINT_USE_FLAT_CONFIG=false npx eslint src/'
            }
        }

        stage('Testes') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        always {
            cleanWs() // Limpa o workspace após a execução
        }
        // 3. Notificações de log solicitadas no enunciado
        success {
            echo 'Pipeline executada com sucesso!'
        }
        failure {
            echo 'A pipeline falhou em um dos estágios de verificação.'
        }
    }
}