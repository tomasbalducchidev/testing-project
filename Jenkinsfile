pipeline {
  agent any

  tools {
    nodejs 'Node 20'
  }

  environment {
    EC2_USER = 'ubuntu'
    EC2_IP = '3.135.207.8'
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
        sh '''
          echo "Instalando dependencias"
          npm install -g @angular/cli@18
          npm ci
          node --max_old_space_size=2500
        '''
      }
    }

    stage('Compilar Angular') {
      steps {
        sh 'ng build --configuration=production --verbose'
      }
    }

    stage('Deploy en EC2') {
      steps {
        sh 'scp -i $SSH_KEY -r dist/* $EC2_USER@$EC2_IP:$EC2_PATH'
      }
    }
  }
}
