pipeline {
    agent any

    stages {
        stage('Checkout') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}