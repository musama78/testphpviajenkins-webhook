properties([pipelineTriggers([githubPush()])])
def tag
def imageWithTag
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
    /* Building and Tagging */
    stage('Build and Tag Image') {
      steps {
        script {
          //def tag = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
          tag = sh(script: "date +%Y%m%d-%H%M", returnStdout: true).trim()
          imageWithTag = "${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"
          // build image
          sh "docker build -t ${IMAGE_NAME}:${tag} ."
          echo "-------------------- Image Build Done --------------------"
          // tag image
          sh "docker tag ${IMAGE_NAME}:${tag} ${imageWithTag}"
          echo "-------------------- Image Tag Done --------------------"
        }
      }
    }
    /* Pushing */
    stage('Push Image') {
      steps {
        script {
          // Docker Login
          sh "docker login -u muhammadusama7 -p Usama1191"
          echo "-------------------- Docker Login Done --------------------"
          
          // push image
          sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${tag}"
          echo "-------------------- Image Push Done --------------------"
        }
      }
    }
    /* Running Container */
    stage ('Run Docker Compose') {
      steps{
        sh 'sudo docker-compose up -d --build --remove-orphans --force-recreate --no-deps --name mycontainer --env TAGVAR=${tag}'
        //sh 'sudo docker-compose up -d'
      }
    }
  }
}
