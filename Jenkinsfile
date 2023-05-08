properties([pipelineTriggers([githubPush()])])

pipeline {
  agent {
    label "abc"
  }
  environment {
    IMAGE_NAME = "my-image"
    DOCKER_REGISTRY = "docker.io"
  }
  stages {
            /* checkout repo */
    stage('Checkout mySCM') {
      steps {
        checkout scmGit(
          branches: [[name: '*/main']], 
          extensions: [], 
          userRemoteConfigs: [[url: 'https://github.com/musama78/testphpviajenkins-webhook.git']]
        )
      }
//       steps {
//                 checkout([
//                  $class: 'GitSCM',
//                  branches: [[name: 'master']],
//                  userRemoteConfigs: [[
//                     //url: 'git@github.com:wshihadeh/rabbitmq_client.git',
//                    url: 'https://github.com/musama78/testphpviajenkins-webhook.git',
//                     credentialsId: '',
//                  ]]
//                 ])
//             }
    }
    ///////////////////////////////////////////////////////
    stage('Build and Push Image') {
      steps {
        script {
          def tag = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
          def imageWithTag = "${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"
          
          // build image
          sh "docker-compose build --build-arg IMAGE_TAG=${tag}"
          
          // tag and push image
          sh "docker tag ${IMAGE_NAME}:latest ${imageWithTag}"
          sh "docker push ${imageWithTag}"
        }
      }
    }
    //////////////////////////////////////////////////////
    stage ('Run Docker Compose') {
      steps{
        sh 'sudo docker-compose up -d'
      }
    }
  }
}
