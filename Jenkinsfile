pipeline {
    
    agent none

    environment {
        COMPOSE_PROJECT_NAME = "${env.JOB_NAME}-${env.BUILD_ID}"
    }
    stages {
      
        stage('Build') {

            agent any

            steps {
                
                echo sh(script: 'env|sort', returnStdout: true)

                sh  '''

                    docker build . -f ./WebApplication1/Dockerfile -t registry.oragon.io/demo/app_demo_1:${BRANCH_NAME:-master}

                    docker push registry.oragon.io/demo/app_demo_1:${BRANCH_NAME:-master}

                '''

            }

        }

        stage('Publish') {

            agent any

            when { buildingTag() }


            steps {
                
                sh  '''

                    docker service update --image registry.oragon.io/demo/app_demo_1:${BRANCH_NAME:-master} demo

               
                '''

            }

        }


 
    }
    post {

        always {
            node('master'){
                
                sh  '''
               
                '''
            }
        }
    }
}