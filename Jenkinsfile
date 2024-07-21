pipeline {
    agent any

    environment {
        
        
    }

    stages {
        
        stage ('Deploy to AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                }
            }
            
            environment  {
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY' )])     
                    sh '''
                    aws --version
                    aws ecs register-task-definition --cli-input-json file://aws/task-definition.json
                    '''
            }
        } 

        
   

       
        
    
        

     
    }
}
