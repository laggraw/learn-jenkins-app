pipeline {
        agent any
    //   agent {
    //             docker {
    //                 image 'node:18-alpine'
    //                 reuseNode true
    //             }
    //         }

    stages {
        // stage('Build') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         sh '''
        //             echo "Build stage started"
        //             ls -la
        //             node --version
        //             npm --version
        //             npm ci
        //             npm run build
        //             ls -la
        //         '''
        //     }
        // }

        stage("Tests") {
            parallel {

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
                            post{
                                    always {
                                        echo 'Unit Test post Execution stage  '
                                        junit 'jest-results/junit.xml'
                                   
                                    }
                                } 

                        }

                stage('E2E') {
                            agent {
                                docker {
                                    // image 'mcr.microsoft.com/playwright:v1.64.0-noble'
                                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                                    reuseNode true
                                }
                            }
                            steps {
                                sh '''
                                    echo "E2E stage started"   
                                    npm install serve
                                node_modules/.bin/serve -s build  &
                                    sleep 10
                                    npx playwright test --reporter=line
                                
                                '''
                            }
                            post{
                                    always {
                                        echo 'E2E post Execution stage'
                                      
                                        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                                    }
                                } 

                        }
            }
        }
        
    }
       
}
