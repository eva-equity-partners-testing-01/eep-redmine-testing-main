pipeline {
    agent any

    environment {
        IMAGE_NAME   = "fastapi-demo"
        IMAGE_TAG    = "${BUILD_NUMBER}"
        GITHUB_TOKEN = credentials('github-token')
        GITHUB_REPO  = "eva-equity-partners-testing-01/eep-redmine-testing-main"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Report Pending') {
            steps {
                script {
                    if (env.SHA) {
                        sh """
                            curl -s -X POST \\
                            -H "Authorization: token ${GITHUB_TOKEN}" \\
                            -H "Content-Type: application/json" \\
                            -d '{"state":"pending","context":"continuous-integration/jenkins","description":"Build in progress...","target_url":"${BUILD_URL}"}' \\
                            https://api.github.com/repos/${GITHUB_REPO}/statuses/${SHA}
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('fastapi-demo') {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

    }

    post {
        success {
            echo "✅ Build #${BUILD_NUMBER} SUCCESS"
            script {
                if (env.SHA) {
                    sh """
                        curl -s -X POST \\
                        -H "Authorization: token ${GITHUB_TOKEN}" \\
                        -H "Content-Type: application/json" \\
                        -d '{"state":"success","context":"continuous-integration/jenkins","description":"Build passed! Ready to merge.","target_url":"${BUILD_URL}"}' \\
                        https://api.github.com/repos/${GITHUB_REPO}/statuses/${SHA}
                    """
                }
                sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
            }
        }

        failure {
            echo "❌ Build #${BUILD_NUMBER} FAILED"
            script {
                if (env.SHA) {
                    sh """
                        curl -s -X POST \\
                        -H "Authorization: token ${GITHUB_TOKEN}" \\
                        -H "Content-Type: application/json" \\
                        -d '{"state":"failure","context":"continuous-integration/jenkins","description":"Build failed! Fix before merging.","target_url":"${BUILD_URL}"}' \\
                        https://api.github.com/repos/${GITHUB_REPO}/statuses/${SHA}
                    """
                }
            }
        }
    }
}
