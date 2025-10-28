pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                echo '🧹 Cleaning workspace...'
                deleteDir()
            }
        }

        stage('Check Git') {
            steps {
                echo '📥 Checking Git Version'
                sh 'git -v'
            }
        }

        stage('Get Repo') {
            steps {
                echo '📥 Cloning the Git repository...'
                sh 'git clone https://github.com/djabirpc/dotnet.docker.sogral'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                script {
                    def buildContext = "${env.WORKSPACE}/dotnet.docker.sogral"

                    // 🧩 Check if Dockerfile exists
                    sh "ls -l ${buildContext}"

                    // 🏗️ Build Docker image (make sure the file is 'Dockerfile' lowercase)
                    def app = docker.build(
                        "sogral-app-api:latest",
                        "-f ${buildContext}/Dockerfile ${buildContext}"
                    )

                    echo "✅ Docker image built successfully: ${app.id}"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🚀 Running Docker container...'
                script {
                    // Stop and remove old container if it exists
                    sh "docker stop sogral-app-api || true"
                    sh "docker rm sogral-app-api || true"

                    // Run new container
                    sh """
                        docker run -d \
                            --name sogral-app-api \
                            -p 8080:8080 \
                            sogral-app-api:latest
                    """

                    echo "✅ Container started at http://localhost:8080"
                }
            }
        }
    }
}
