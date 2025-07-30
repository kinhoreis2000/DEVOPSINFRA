pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN') 
    }

    stages {
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