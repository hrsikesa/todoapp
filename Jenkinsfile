pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // YOUR_DOCKERHUB_USERNAME
     // YOUR_DOCKERHUB_PASSWORD 

     SERVICE_NAME = "todoapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${SERVICE_NAME}"
         }
      }
      
      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
           sh 'docker login -u $YOUR_DOCKERHUB_USERNAME -p $YOUR_DOCKERHUB_PASSWORD'
           sh 'docker push ${REPOSITORY_TAG}'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
