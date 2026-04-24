pipeline {
    agent any

    stages {

        stage('Deployment Start Notification') {
            steps {
                echo 'STEP 1: Deployment Start Notification'
            }
        }

        stage('Git Checkout') {
            steps {
                echo 'STEP 2: Git Checkout'
            }
        }

        stage('SonarQube Scanner') {
            steps {
                echo 'STEP 3: SonarQube Scanner'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'STEP 4: Docker Build'
            }
        }

        stage('Docker Image Push to ECR') {
            steps {
                echo 'STEP 5: Docker Image Push to ECR'
            }
        }

        stage('Kubectl Set Image') {
            steps {
                echo 'STEP 6: Kubectl Set Image'
            }
        }

        stage('Deployment Completed Notification') {
            steps {
                echo 'STEP 7: Deployment Completed Notification'
            }
        }

    }
}
