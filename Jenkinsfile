
pipeline {
   agent any

   environment {
     SERVICE_NAME = "webserver"
     REPOSITORY_NAME = "k8s-jenkins-cd"
     IMAGE_TAG="${DOCKERHUB_USERNAME}/${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${REPOSITORY_NAME}/"
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${IMAGE_TAG} .'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}