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
                    echo "logging files and node versions"
                    ls -la
                    node -v  
                    npm -v   
                    echo "Ensuring PNPM is available"
                    corepack enable
                    corepack prepare pnpm@latest --activate
                    cd ${WORKSPACE}
                    echo "Installing dependencies"
                    pnpm install --frozen-lockfile
                    
                    echo "Building project"
                    pnpm build  
                '''
            }
        }
    }
}
