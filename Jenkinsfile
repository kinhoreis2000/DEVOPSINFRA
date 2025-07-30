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
            echo '• Staging: https://stagingfinal-cb7de.web.app'
            echo '• Production: [URL will show after deploy]'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}