properties([pipelineTriggers([githubPush()])])

pipeline {
  agent {
    label "abc"
  }
  environment {
    IMAGE_NAME = "phpcicd"
    DOCKER_REGISTRY = "muhammadusama7"
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
    stage('*******************Build Image*******************') {
      steps {
        script {
          def tag = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
          def imageWithTag = "${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"
          
          // build image
          sh "docker build . -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"

        }
      }
    }
    //////////////////////////////////////////////////////
    stage('*******************Push Image*******************') {
      steps {
        script {
         
          // tag and push image
          sh "docker tag ${IMAGE_NAME}:latest ${imageWithTag}"
          sh "docker push ${imageWithTag}"
          
          sh "export myTAG=${tag}"
        }
      }
    }
    /////////////////////////////////////////////////////
    stage ('Run Docker Compose') {
      steps{
        sh 'sudo docker-compose up -d --build --remove-orphans --force-recreate --no-deps --name mycontainer --env TAG=${tag}'
      }
    }
  }
}
