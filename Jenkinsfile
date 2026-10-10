pipeline {
      agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

    stages {
        stage('Build') {
            // agent {
            //     docker {
            //         image 'node:18-alpine'
            //         reuseNode true
            //     }
            // }
            steps {
                sh '''
                    echo "Build stage started"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            // agent {
            //     docker {
            //         image 'node:18-alpine'
            //         reuseNode true
            //     }
            // }
            steps {
                sh '''
                    echo "Test stage started"   
                  test -f 'build/index.html'
                    npm test    
                    ls -la
                '''
            }
        }
    }

       post{
           always {
               echo 'Test stage completed'
               junit 'test-results/junit.xml'
           }
       } 
}
