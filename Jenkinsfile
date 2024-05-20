pipeline {
    agent any

    environment {
        DOTNET_ROOT = tool name: 'dotnet-sdk' // Ensure .NET SDK is configured in Jenkins global tools
        PATH = "${DOTNET_ROOT}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mahimakumari123/MyDotNetApp.git'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore MyDotNetApp/MyDotNetApp.csproj'
                sh 'dotnet restore MyDotNetApp.Tests/MyDotNetApp.Tests.csproj'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build MyDotNetApp/MyDotNetApp.csproj --no-restore'
                sh 'dotnet build MyDotNetApp.Tests/MyDotNetApp.Tests.csproj --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test MyDotNetApp.Tests/MyDotNetApp.Tests.csproj --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/bin/**/*.dll', allowEmptyArchive: true
            junit 'MyDotNetApp.Tests/TestResults/*.xml'
        }
    }
}
