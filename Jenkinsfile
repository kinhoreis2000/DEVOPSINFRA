pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN')
    }

    stages {
        stage('Build - Install Firebase Tools') {
            steps {
                echo 'Installing Firebase CLI...'
                sh 'npm install -g firebase-tools'
                sh 'firebase --version'
                echo 'Firebase CLI installed successfully!'
            }
        }

        stage('Deploy to Testing') {
            steps {
                echo 'Deploying to Testing environment...'
                sh 'firebase use testing --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
                echo 'Testing deployment completed!'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging environment...'
                sh 'firebase use staging --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
                echo 'Staging deployment completed!'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Ready to deploy to Production...'
                input message: 'Deploy to Production?', ok: 'Deploy'
                echo 'Deploying to Production environment...'
                sh 'firebase use production --token "$FIREBASE_TOKEN"'
                sh 'firebase deploy --only hosting --token "$FIREBASE_TOKEN"'
                echo 'Production deployment completed!'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
            echo 'All environments deployed:'
            echo '• Testing: https://testingfinal-5a606.web.app'
            echo '• Staging: https://staging-project.web.app'
            echo '• Production: https://production-project.web.app'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}