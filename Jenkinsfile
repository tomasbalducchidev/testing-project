pipeline {
  agent any

  tools {
    nodejs 'Node 20'
  }

  environment {
    EC2_USER = 'ubuntu'
    EC2_IP = '3.147.242.49'
    EC2_PATH = '/var/www/html'
    SSH_KEY = '/var/lib/jenkins/.ssh/ng-testing-keys.pem'
  }

  stages {
    stage('Clonar repositorio') {
      steps {
        git url: 'https://github.com/tomasbalducchidev/testing-project', branch: 'main'
      }
    }

    stage('Instalar dependencias') {
      steps {
        sh 'npm install'
      }
    }

    stage('Compilar Angularr') {
      steps {
        sh 'export NODE_OPTIONS="--max-old-space-size=2048"'
        sh 'ng build --configuration=production'
      }
    }

    stage('Deploy en EC2') {
      steps {
        sh 'scp -i $SSH_KEY -r dist/* $EC2_USER@$EC2_IP:$EC2_PATH'
      }
    }
  }
}
