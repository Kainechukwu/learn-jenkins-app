void installPNPM() {
    def isPNPMInstalled = sh(script: 'which pnpm || echo "not_found"', returnStdout: true).trim()
    if (isPNPMInstalled == "not_found") {
        echo 'pnpm not found, installing...'
        sh 'npm install -g pnpm --force --unsafe-perm=true'
    } else {
        echo 'pnpm already installed'
    }

    sh 'pnpm --version || echo "pnpm is not installed"'
}

void pnpmInstall() {
    sh 'pnpm install || (echo "pnpm install failed" && exit 1)'
}

void pnpmBuild() {
    sh 'pnpm build || (echo "Build failed" && exit 1)'
}

pipeline {
    agent any

    stages {
        stage('Setup & Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root' // Ensure permissions are not an issue
                }
            }

            steps {
                sh '''
                    echo "🔍 Logging files and versions"
                    ls -la
                    node -v
                    npm -v
                '''
                script {
                    installPNPM()
                    pnpmInstall()
                    pnpmBuild()
                }
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }

            steps {
                sh '''
                    echo "🧪 Running tests"
                    test -f build/index.html && echo "index.html found" || (echo "index.html NOT found" && exit 1)
                    pnpm test || (echo "Tests failed" && exit 1)
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
