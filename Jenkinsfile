pipeline {
    agent any

    environment {
        DOCKER_IMAGE   = 'mynandhagopalapp'
        DOCKER_TAG     = "${BUILD_NUMBER}"
        CONTAINER_NAME = 'mynandhagopalapp-container'
        APP_PORT       = '8080'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '>>> Pulling code from GitHub...'
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                echo '>>> Restoring NuGet packages...'
                bat 'dotnet restore src/MyApp/MyApp.csproj'
            }
        }

        stage('Build') {
            steps {
                echo '>>> Building .NET Core app...'
                bat 'dotnet build src/MyApp/MyApp.csproj --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo '>>> Running unit tests...'
                bat 'dotnet test tests/MyApp.Tests/MyApp.Tests.csproj --no-restore --verbosity normal'
            }
        }

        stage('Docker Build') {
            steps {
                echo '>>> Building Docker image...'
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
                bat "docker tag %DOCKER_IMAGE%:%DOCKER_TAG% %DOCKER_IMAGE%:latest"
            }
        }

        stage('Docker Deploy') {
            steps {
                echo '>>> Stopping old container if running...'
                bat "docker stop %CONTAINER_NAME% || exit 0"
                bat "docker rm %CONTAINER_NAME% || exit 0"

                echo '>>> Starting new container...'
                bat "docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:80 --restart unless-stopped %DOCKER_IMAGE%:%DOCKER_TAG%"

                echo '>>> Deployment complete!'
            }
        }

    }

    post {
        success {
            echo '✅ Pipeline SUCCESS — App deployed!'
        }
        failure {
            echo '❌ Pipeline FAILED — Check logs!'
        }
        always {
            bat "docker image prune -f || exit 0"
        }
    }
}