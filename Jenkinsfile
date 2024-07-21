pipeline {
    agent any

    environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        
        stage ('Deploy to AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "--entrypoint=''"
                }
            }
            
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY' )]) {    
                    sh '''
                    aws --version
                    aws ecs register-task-definition --cli-input-json file://aws/task-definition.json
                    '''
                }    
            }
        } 
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}     
