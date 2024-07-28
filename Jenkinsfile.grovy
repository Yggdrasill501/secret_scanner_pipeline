pipeline {
    agent any
    
    environment {
        BITBUCKET_REPO = 'https://bitbucket.org/yourusername/yourrepo.git'
        CREDENTIALS_ID = 'your-bitbucket-credentials-id'
        ARTIFACTORY_SERVER_ID = 'your-artifactory-server-id'
        ARTIFACTORY_REPO = 'your-artifactory-repo'
        ARTIFACTORY_CREDENTIALS_ID = 'your-artifactory-credentials-id'
        GITLEAKS_TOOL = 'gitleaks'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: env.BITBUCKET_REPO,
                    credentialsId: env.CREDENTIALS_ID
            }
        }
        
        stage('Scan for Secrets') {
            steps {
                script {
                    try {
                        sh "${env.GITLEAKS_TOOL} detect --source . --exit-code 1"
                    } catch (Exception e) {
                        error("Secrets found in the codebase!")
                    }
                }
            }
        }
        
        stage('Build Artifact') {
            steps {
                // Example build step, replace with your build tool commands
                sh 'mvn clean install'
            }
        }
        
        stage('Upload to JFrog Artifactory') {
            steps {
                script {
                    def server = Artifactory.server(env.ARTIFACTORY_SERVER_ID)
                    def uploadSpec = """{
                        "files": [{
                            "pattern": "target/*.jar",
                            "target": "${env.ARTIFACTORY_REPO}"
                        }]
                    }"""
                    server.upload(uploadSpec)
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
