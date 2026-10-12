pipeline {
        agent any
    //   agent {
    //             docker {
    //                 image 'node:18-alpine'
    //                 reuseNode true
    //             }
    //         }
    environment {
        NETLIFY_SITE_ID = '4ae7303d-8444-4ad2-af1a-bd747a6cc646'
    }

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
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "DEPLOYING TO $NETLIFY_SITE_ID"
                  
               
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
