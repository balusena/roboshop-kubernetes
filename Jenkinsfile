pipeline {
  agent {
    node {
      label 'workstation'
    }
  }

  parameters {
        string(name: 'COMPONENT', defaultValue: '', description: 'Which Component')
        string(name: 'ENV', defaultValue: 'prod', description: 'Which Env')
        string(name: 'APP_VERSION', defaultValue: '', description: 'Which Version')

  }

  stages {
    stage('Clone App Repo') {
      steps {
        dir('APP') {
          git branch: 'main', url: 'https://github.com/balusena/${COMPONENT}'
        }
      }
    }

    stage('Helm Chart Deploy') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-eks'
        sh 'helm upgrade -i ${COMPONENT} roboshop -f APP/helm.yaml --set image.tag=${APP_VERSION}'
      }

    }
  }

  post {
    always {
      cleanWs()
    }
  }

}