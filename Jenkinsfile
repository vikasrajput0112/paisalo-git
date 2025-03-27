pipeline {
    agent any
    
    environment {
        // Define environment variables for SonarQube and Snyk tokens if required
        SONARQUBE_TOKEN = credentials('sonar-token') // Your SonarQube token stored in Jenkins credentials
        SNYK_TOKEN = credentials('snyk-token')       // Snyk token if using Snyk for security scanning
    }

    tools {
        // Use the .NET SDK installed on your Jenkins agent
        dotnet 'dotnet'  // Name of the .NET SDK installation in Jenkins (adjust accordingly)
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from Git repository...'
                git 'https://github.com/vikasrajput0112/paisalo-git.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                echo 'Restoring NuGet dependencies...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the .NET project...'
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'dotnet test --configuration Release --no-build --verbosity normal'
            }
        }

        stage('Run Lint (StyleCop)') {
            steps {
                echo 'Running Lint checks (StyleCop)...'
                // If using StyleCop or another tool for linting
                sh 'dotnet tool install --global StyleCop.Analyzers'
                sh 'dotnet build -f netcoreapp3.1 --no-restore --analyze'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                script {
                    // Run SonarQube analysis
                    sh """
                        dotnet sonarscanner begin /k:"my-dotnet-project" /d:sonar.login=$SONARQUBE_TOKEN
                        dotnet build
                        dotnet sonarscanner end /d:sonar.login=$SONARQUBE_TOKEN
                    """
                }
            }
        }

        stage('Security Scan (Snyk)') {
            steps {
                echo 'Running security scan with Snyk...'
                script {
                    // Run security scan with Snyk (if integrated)
                    sh 'snyk test --all-projects --token=$SNYK_TOKEN'
                }
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing the .NET project...'
                sh 'dotnet publish --configuration Release --output ./publish'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to environment (e.g., Azure or IIS)...'
                // Add your deployment commands here, like Azure Web App deployment or other server deployments
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
