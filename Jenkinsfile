properties([pipelineTriggers([githubPush()])])

pipeline {
  agent {
    label "abc"
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
    stage ('Run Docker Compose') {
      steps{
        sh 'sudo docker-compose up -d'
      }
    }
  }
}
