pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Logging initial info"
                    ls -la
                    node --version
                    npm --version

                    echo "Installing pnpm"
                    npm install -g pnpm --force

                    echo "Installing dependencies with pnpm"
                    pnpm install

                    echo "Running build"
                    pnpm run build

                    echo "Post-build file list"
                    ls -la
                '''
            }
        }
    }
}
