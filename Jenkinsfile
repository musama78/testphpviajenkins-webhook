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
    }
    ///////////////////////////////////////////////////////
    stage('Build and Push Image') {
      steps {
        script {
          def tag = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
          def imageWithTag = "${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"
          //////////////////////
            sh "docker build -t ${IMAGE_NAME}:${tag} ."
            //sh "docker tag ${IMAGE_NAME}:${tag} ${IMAGE_REPO}/${NAME}:${VERSION}"
          //////////////////////
//////////// build image
          //sh "docker-compose build --build-arg IMAGE_TAG=${tag}"
          //sh "docker-compose build . -t ${IMAGE_NAME}:${tag}"
          
//////////// tag and push image
          sh "docker tag ${IMAGE_NAME}:${tag} ${imageWithTag}"
          //sh "docker push ${imageWithTag}"
        }
      }
    }
    //////////////////////////////////////////////////////
//     stage ('Run Docker Compose') {
//       steps{
//         sh 'sudo docker-compose up -d --build --remove-orphans --force-recreate --no-deps --name mycontainer --env TAGVAR=${tag}'
//         sh 'sudo docker-compose up -d'
//       }
//     }
  }
}
