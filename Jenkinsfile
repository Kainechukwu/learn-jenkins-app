pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    stages {
       
        stage('Build') {
          
            steps {
                sh '''
                    echo "logging files and node versions"
                    ls -la
                    node -v  
                    npm -v   
                    npm ci
                    npm run build 
                    ls -la 
                '''
            }
        }
        stage('Test') {     
            
            steps {
                sh '''
                    echo "Test stage"
                   # npm ci
                   # npm run test 
                   test -f build/index.html && echo "index.html found" || (echo "index.html NOT found" && exit 1)
                   
                '''
            }
        }
   
    }
}
