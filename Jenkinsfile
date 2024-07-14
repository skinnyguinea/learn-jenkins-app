pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'e0cefa3e-196d-44a9-8d41-e84b996d7a1d'

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
        stage ('Deploy')  {
            agent {
                docker {
                  image: 'node:18-alpine'
                  reuseNode: true
                }
            }  
            steps {
              sh '''
               npm install netlify-cli
               node_modules/.bin/netlify --version
               echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
               '''
            }   
        }    
    }
}
        
  
    

        
    
