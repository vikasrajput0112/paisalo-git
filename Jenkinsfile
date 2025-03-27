pipeline {
    agent any

       stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building .NET Project...'
                    sh 'dotnet restore'
                    sh 'dotnet build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running Tests...'
                    sh 'dotnet test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying .NET Project...'
                    // You can replace this with your actual deployment steps
                    sh 'dotnet publish -c Release -o ./publish'
                    // Example: upload to a server or deploy to Azure/AWS, etc.
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
