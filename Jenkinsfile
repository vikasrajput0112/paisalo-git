pipeline {
    agent any
    tools {
        dotnet 'dotnet'  // Ensure this matches the name of the .NET SDK tool in Jenkins Global Tool Configuration
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'dotnet build'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'dotnet test'
                }
            }
        }
    }
}
