pipeline {
    agent any

    environment {
        DOTNET_ROOT = '/usr/local/share/dotnet'
        PATH = "${DOTNET_ROOT}:${PATH}"
    }

    stages {
        stage('Skip Irrelevant Branches') {
            when {
                expression {
                    def branch = env.BRANCH_NAME
                    return branch == 'main' || branch.startsWith('feature/')
                }
            }
            steps {
                echo "Proceeding with build for branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Restore') {
            when {
                expression {
                    def branch = env.BRANCH_NAME
                    return branch == 'main' || branch.startsWith('feature/')
                }
            }
            steps {
                echo 'Restoring dependencies...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            when {
                expression {
                    def branch = env.BRANCH_NAME
                    return branch == 'main' || branch.startsWith('feature/')
                }
            }
            steps {
                echo 'Building the solution...'
                sh 'dotnet build --no-restore'
            }
        }

        stage('Rebuild') {
            when {
                expression {
                    def branch = env.BRANCH_NAME
                    return branch == 'main' || branch.startsWith('feature/')
                }
            }
            steps {
                echo 'Rebuilding the solution...'
                sh 'dotnet build --no-restore --no-incremental'
            }
        }

        stage('Test') {
            when {
                expression {
                    def branch = env.BRANCH_NAME
                    return branch == 'main' || branch.startsWith('feature/')
                }
            }
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
            echo '❌ Pipeline failed. Please check the logs!'
        }
    }
}
