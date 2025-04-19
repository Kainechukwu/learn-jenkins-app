void installPNPM() {
    def isPNPMInstalled = sh(script: 'command -v pnpm || true', returnStdout: true).trim()
    if (!isPNPMInstalled) {
        echo 'pnpm not found, installing...'
        sh 'npm install -g pnpm --force'
    } else {
        echo 'pnpm already installed'
    }

    sh 'pnpm --version'
}

void pnpmInstall() {
    sh 'pnpm install --frozen-lockfile'
}

void pnpmBuild() {
    sh 'pnpm build'
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
                    pnpm test
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
