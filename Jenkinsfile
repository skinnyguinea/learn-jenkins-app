pipeline {
    agent any
}

    stages {
        stage('Build') {
        // This is a comment    
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                sh '''
                  ls -la 
                  node --version
                  npm --version
                  npm ci 
                  npm run build
                '''  
            }            
        }

        stage(RunTests) {
            parallel {
                 stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
               sh '''
                  echo "Test Stage"
                  test -f build/index.html
                  npm test 
                '''
           }
        }
            stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                    args '-u root:root'
                }
            }
            steps {
                sh '''
                npm install serve
                node_modules/.bin/serve -s build &
                sleep 10 
                npx playwright test
                ''' 
            } 
        }
     }
   } 
}
        
    
