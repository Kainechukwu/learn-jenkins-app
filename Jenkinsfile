void installPNPM() {
    def status = sh(script: 'command -v pnpm || true', returnStdout: true).trim()

    if (!status) {
        sh 'npm install -g pnpm --force'
    }
}

void pnpmInstall() {
    sh 'pnpm install'
 
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
                    echo "Test stage"
                  
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
