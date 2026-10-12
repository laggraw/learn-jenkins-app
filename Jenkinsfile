pipeline {
        agent any
    //   agent {
    //             docker {
    //                 image 'node:18-alpine'
    //                 reuseNode true
    //             }
    //         }

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


        stage(' Unit Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Test stage started"   
                test -f 'build/index.html'
                    npm test    
                    ls -la
                '''
            }
        }
            
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Deploy stage started"
                    npm install -g netlify-cli
                    netlify --version
               
                '''
            }
        }
        
        }

     post{
        always {
            echo 'Unit Test post Execution stage  '
            junit 'jest-results/junit.xml'      
            }
        } 
       
}
