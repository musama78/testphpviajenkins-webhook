properties([pipelineTriggers([githubPush()])])
def tag
def imageWithTag
pipeline {
//  agent any
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
    stage('Removing Previous Images and Containers') {
      steps {
        script {
          // Removing Containers
          def containerName = sh(script: "sudo docker ps -aqf 'name=phpcicd'", returnStdout: true).trim()
          if (containerName) {
            sh "sudo docker rm -f ${containerName}"
            echo "++++++++++++++++++++ Previous Containers Removed ++++++++++++++++++++"
          }
          
          // Removing Images
          def imageExists = sh(script: "docker images | grep phpcicd", returnStatus: true) == 0
          if (imageExists) {
            sh "docker rmi -f \$(docker images -q '*phpcicd*')"
            echo "++++++++++++++++++++ Previous Images Removed ++++++++++++++++++++"
          }
        }
      }
    }
    /* Building and Tagging. */
    stage('Build and Tag Image') {
      steps {
        script {
          //def tag = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
          tag = sh(script: "date +%Y%m%d-%H.%M.%S", returnStdout: true).trim()
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
          sh "docker push ${imageWithTag}"
          echo "-------------------- Image Push Done --------------------"
        }
      }
    }
    /* Running Container */
    stage ('Creating Container') {
      steps{
        //sh 'sudo docker-compose up -d  --name mycontainer --env TAGVAR=${tag}'
        //--build --remove-orphans --force-recreate --no-deps
        //sh 'sudo docker-compose up -d'
        script {
         
            sh "sudo docker run -d -p 80:80 --name ${IMAGE_NAME}-${tag} ${imageWithTag}"

          //sh "docker container ls --filter name=phpcicd-* -q | xargs docker container rm -f"
          //sh "sudo docker run -d -p 80:80 --name ${IMAGE_NAME}-${tag} ${imageWithTag}"
          echo "-------------------- Container Deployment Done --------------------"
        }
      }
    }
  }
}
