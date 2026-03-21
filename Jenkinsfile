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
                echo '>>> Step 1: Pulling code from GitHub...'
                checkout scm
            }
        }

        stage('Check Environment') {
            steps {
                echo '>>> Step 2: Checking tools...'
                bat 'dotnet --version'
                bat 'docker --version'
                bat 'dir'
            }
        }

        stage('Restore') {
            steps {
                echo '>>> Step 3: Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo '>>> Step 4: Building app...'
                bat 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo '>>> Step 5: Running tests...'
                bat 'dotnet test --no-restore --verbosity normal'
            }
        }

        stage('Docker Build') {
            steps {
                echo '>>> Step 6: Building Docker image...'
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
                bat "docker tag %DOCKER_IMAGE%:%DOCKER_TAG% %DOCKER_IMAGE%:latest"
            }
        }

        stage('Docker Deploy') {
            steps {
                echo '>>> Step 7: Deploying container...'
                bat "docker stop %CONTAINER_NAME% || exit 0"
                bat "docker rm %CONTAINER_NAME% || exit 0"
                bat "docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:80 --restart unless-stopped %DOCKER_IMAGE%:%DOCKER_TAG%"
                echo '>>> App running on port 8080!'
            }
        }

    }

    post {
        success { echo '✅ SUCCESS — App deployed on port 8080!' }
        failure { echo '❌ FAILED — Check console output above!' }
        always  { bat "docker image prune -f || exit 0" }
    }
}