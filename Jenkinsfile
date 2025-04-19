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
                    npm ci
                    pnpm run build 
                    ls -la 
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
