pipeline {
    agent any

    environment {
        GITHUB_API_URL = 'https://api.github.com/user/repos'
        GITHUB_PAT = credentials('YourGitHubPAT')
        REPO_NAME = 'my-new-repo'
        REPO_DESCRIPTION = 'My new GitHub repository created by Jenkins'
    }

    stages {
        stage('Create GitHub Repository') {
            steps {
                script {
                    def response = httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_JSON',
                        httpMode: 'POST',
                        url: GITHUB_API_URL,
                        authentication: 'GITHUB_PAT',
                        requestBody: [
                            name: REPO_NAME,
                            description: REPO_DESCRIPTION,
                            private: false  // Set to 'true' for a private repository
                        ]
                    )

                    if (response.status == 201) {
                        echo "GitHub repository created successfully!"
                    } else {
                        error "Failed to create GitHub repository. Status code: ${response.status}"
                    }
                }
            }
        }
    }
}
