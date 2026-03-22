# 🚀 .NET Core CI/CD Pipeline with Jenkins & Docker

![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-Source-181717?style=for-the-badge&logo=github&logoColor=white)

---

## 📋 Project Overview

This project demonstrates a fully automated **CI/CD pipeline** using **Jenkins** for a **.NET Core 10 Web API** application, containerized with **Docker**. The pipeline automatically builds, tests, and deploys the application whenever code is pushed to the repository.

---

## 🏗️ Architecture

```
Developer Push Code
        ↓
    GitHub Repo
        ↓
  Jenkins Pipeline
        ↓
┌───────────────────────────────────────┐
│  Stage 1: Checkout  → Pull from GitHub │
│  Stage 2: Restore   → NuGet Packages  │
│  Stage 3: Build     → Release Build   │
│  Stage 4: Test      → xUnit Tests     │
│  Stage 5: Docker Build → Docker Image │
│  Stage 6: Docker Deploy → Container   │
└───────────────────────────────────────┘
        ↓
  App Running on Port 8080
```

---

## 🛠️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| **.NET 10** | Web API Application |
| **Jenkins** | CI/CD Automation |
| **Docker** | Containerization |
| **xUnit** | Unit Testing |
| **GitHub** | Source Code Management |

---

## 📁 Project Structure

```
Testing_app/
├── Jenkinsfile                          ← CI/CD Pipeline definition
├── Dockerfile                           ← Docker image configuration
├── docker-compose.yml                   ← Local development setup
├── src/
│   └── MyApp/
│       ├── MyApp.csproj                 ← Project file
│       ├── Program.cs                   ← App entry point
│       └── Controllers/
│           └── HomeController.cs        ← API Controller
└── tests/
    └── MyApp.Tests/
        ├── MyApp.Tests.csproj           ← Test project file
        └── UnitTest1.cs                 ← Unit tests
```

---

## ⚙️ CI/CD Pipeline Stages

### Stage 1 — Checkout
```groovy
stage('Checkout') {
    steps { checkout scm }
}
```
Pulls the latest code from GitHub repository.

### Stage 2 — Restore
```groovy
stage('Restore') {
    steps { bat 'dotnet restore src/MyApp/MyApp.csproj' }
}
```
Restores all NuGet dependencies.

### Stage 3 — Build
```groovy
stage('Build') {
    steps { bat 'dotnet build src/MyApp/MyApp.csproj --configuration Release' }
}
```
Compiles the application in Release mode.

### Stage 4 — Test
```groovy
stage('Test') {
    steps { bat 'dotnet test tests/MyApp.Tests/MyApp.Tests.csproj' }
}
```
Runs all xUnit unit tests automatically.

### Stage 5 — Docker Build
```groovy
stage('Docker Build') {
    steps { bat 'docker build -t mynandhagopalapp:latest .' }
}
```
Builds a Docker image of the application.

### Stage 6 — Docker Deploy
```groovy
stage('Docker Deploy') {
    steps {
        bat 'docker stop mynandhagopalapp-container || exit 0'
        bat 'docker rm mynandhagopalapp-container || exit 0'
        bat 'docker run -d --name mynandhagopalapp-container -p 8080:80 mynandhagopalapp:latest'
    }
}
```
Stops old container and deploys the new version with zero-downtime strategy.

---

## 🐳 Docker Setup

### Build Image Manually
```bash
docker build -t mynandhagopalapp:latest .
```

### Run Container
```bash
docker run -d --name mynandhagopalapp-container -p 8080:80 mynandhagopalapp:latest
```

### Using Docker Compose
```bash
docker-compose up -d
```

---

## 🚀 Getting Started

### Prerequisites
- .NET 10 SDK
- Jenkins 2.x
- Docker Desktop
- Git

### Run Locally
```bash
# Clone the repository
git clone https://github.com/Nandhagopal22/Testing_app.git
cd Testing_app

# Restore packages
dotnet restore src/MyApp/MyApp.csproj

# Build
dotnet build src/MyApp/MyApp.csproj --configuration Release

# Run tests
dotnet test tests/MyApp.Tests/MyApp.Tests.csproj

# Run the application
dotnet run --project src/MyApp/MyApp.csproj
```

### Access the API
```
http://localhost:8080/home
```

Expected Response:
```json
{
  "message": "Hello from Nandhagopal's .NET Core App!",
  "version": "1.0",
  "build": "1"
}
```

---

## 🧪 Unit Tests

Tests are written using **xUnit** framework:

```csharp
[Fact]
public void BasicTest_ShouldPass()
{
    int result = 5 + 3;
    Assert.Equal(8, result);
}
```

Run tests:
```bash
dotnet test tests/MyApp.Tests/MyApp.Tests.csproj --verbosity normal
```

---

## 🔧 Jenkins Setup

1. Install Jenkins plugins: **Git**, **Pipeline**, **Docker Pipeline**
2. Create **Multibranch Pipeline** job
3. Add GitHub repository URL
4. Add GitHub credentials
5. Jenkins auto-discovers `Jenkinsfile` and runs pipeline

---

## 👨‍💻 Author

**Nandhagopal M**
- 🌐 [LinkedIn](https://linkedin.com/in/nandhagopal)
- 🐙 [GitHub](https://github.com/Nandhagopal22)
- 📧 marimuthunandhagopal@gmail.com

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
