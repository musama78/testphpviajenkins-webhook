pipeline {
  agent {
    label "agent1"
  }
  stages {
            /* checkout repo */
    stage('Checkout SCM') {
      steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    //url: 'git@github.com:wshihadeh/rabbitmq_client.git',
                   url: 'https://github.com/musama78/testphpviajenkins-webhook.git',
                    credentialsId: '',
                 ]]
                ])
            }
    }
    stage ('Run Docker Compose') {
      steps{
        sh 'sudo docker-compose up -d'
      }
    }
  }
}
