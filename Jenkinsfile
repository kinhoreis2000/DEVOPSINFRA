pipeline {
    agent any

    triggers {
        githubPush()
    }

    // ADICIONE ESTA SEÇÃO:
    options {
        // Manter apenas os últimos 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        
        // Timeout do pipeline
        timeout(time: 30, unit: 'MINUTES')
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN') 
    }

    stages {
        // ADICIONE ESTE STAGE PARA DEBUG:
        stage('Debug Info') {
            steps {
                script {
                    echo "Branch atual: ${env.BRANCH_NAME}"
                    echo "Git commit: ${env.GIT_COMMIT}"
                    sh 'git branch -a'
                    sh 'pwd && ls -la'
                }
            }
        }

        stage('Install Firebase CLI') {
            steps {
                sh 'npm install -g firebase-tools'
            }
        }

        stage('Deploy to Testing') {
            steps {
                sh 'firebase use testing --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'firebase use staging --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }

        stage('Deploy to Production') {
            steps {
                input "Deploy to production?"
                sh 'firebase use production --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
            }
        }
    }
}