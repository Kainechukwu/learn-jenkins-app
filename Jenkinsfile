void installPNPM() {
    def status = sh(script: 'command -v pnpm || true', returnStdout: true).trim()

    if (!status) {
        sh 'npm install -g pnpm --force'
    }
}

void pnpmInstall() {
    // sh 'pnpm install'
    sh '''
    npm ci
    npm run build

    
    '''
}

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

                    
                '''
                 script {
                        // installPNPM()
                        pnpmInstall()
                    }
            }
        }
        stage('Test') {   
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }   
            
            steps {
                sh '''
                    echo "Test stagez"
                    test -f build/index.html && echo "index.html found" || (echo "index.html NOT found" && exit 1)
                     npm test 
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
