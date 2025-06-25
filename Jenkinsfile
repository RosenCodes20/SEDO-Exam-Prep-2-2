pipeline {
    agent any

    environment {
        DOTNET_ROOT = '/usr/local/share/dotnet'
        PATH = "${DOTNET_ROOT}:${PATH}"
    }

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Check Branch') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    if (!(branch == 'main' || branch.startsWith('feature/'))) {
                        echo "Skipping build for branch: ${branch}"
                        // Exit pipeline early with SUCCESS status (no failure)
                        currentBuild.result = 'NOT_BUILT'
                        error("Branch ${branch} not built.")
                    }
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                echo 'Restoring dependencies...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the solution...'
                sh 'dotnet build --no-restore'
            }
        }

        stage('Rebuild') {
            steps {
                echo 'Rebuilding the solution...'
                sh 'dotnet build --no-restore --no-incremental'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo "Pipeline completed for branch: ${env.BRANCH_NAME}"
        }
        success {
            echo '✅ Build and tests succeeded!'
        }
        failure {
            if (currentBuild.result == 'NOT_BUILT') {
                echo "⚠️ Pipeline skipped for this branch."
            } else {
                echo '❌ Pipeline failed. Please check the logs.'
            }
        }
    }
}
