**Jenkins Pipeline to Clone GitHub Repository Using Personal Access Token**

This Jenkins pipeline demonstrates how to clone a GitHub repository securely using a Personal Access Token (PAT). By binding the GitHub token securely through Jenkins credentials, this script ensures the token is not exposed in the logs or source code.

**Purpose**
This pipeline is designed to securely clone a GitHub repository onto the Jenkins workspace using a PAT. It also ensures the workspace is clean before cloning, which can prevent issues from leftover files or previous builds.

**Prerequisites**
Jenkins Server: Ensure Jenkins is installed and configured.
GitHub Personal Access Token: Generate a PAT with appropriate repository access permissions on GitHub and store it in Jenkins credentials under an identifier, such as github_token.
Jenkins Git Plugin: Verify that the Git plugin is installed on Jenkins to support Git operations.
GitHub Repository: Have the repository URL ready for cloning.
Pipeline Script
Here’s the Jenkins pipeline script for reference:

**Pipeline Code**

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

**Explanation**
Clean Workspace: The rm -rf * command ensures any previous files in the workspace are removed before cloning, preventing conflicts or leftover data from earlier builds.
Secure Token Binding: withCredentials securely binds the GITHUB_TOKEN from Jenkins credentials so the token isn’t exposed in the source code or logs.
Clone with HTTPS: The repository is cloned over HTTPS using the PAT, making it both secure and convenient without requiring SSH keys or manual authentication.

**Usage**
Add GitHub Token to Jenkins: In Jenkins, navigate to Manage Jenkins > Manage Credentials and add a new credential:
Kind: Secret text
Secret: GitHub PAT
ID: github_token
Run Pipeline: In Jenkins, click Build Now on the job using this pipeline script. Jenkins will securely clone the repository to the workspace.

**Important Notes**
Token Security: Only authorized users should have access to this Jenkins job since it uses a sensitive token.
Workspace Cleaning: The command rm -rf * removes all files in the workspace before cloning. Ensure this is acceptable for your workflow as it deletes any existing files.
Repository Access: The PAT should have at least repo scope permissions on GitHub to allow cloning.
