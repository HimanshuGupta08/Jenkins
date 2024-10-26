pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clean the workspace if necessary
                    sh 'rm -rf *'  // Ensure the workspace is clean

                    // Use withCredentials to bind the token
                    withCredentials([string(credentialsId: 'github_token', variable: 'GITHUB_TOKEN')]) {
                        // Clone the repository using HTTPS and the personal access token
                        sh "git clone https://${GITHUB_TOKEN}@github.com/richest/test-repo.git"
                    }
                }
            }
        }
    }
}
