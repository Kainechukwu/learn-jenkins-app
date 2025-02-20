pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                sh '''
                    echo "loging files and node versions"
                    ls -la
                    node -v  
                    npm -v   
                    pnpm ci
                    pnpm build   
                '''
            }
        }
    }
}
