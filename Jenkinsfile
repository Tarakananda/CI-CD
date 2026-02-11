pipeline {
  agent any

  environment{
    IMAGE_NAME = "creatingproject"
    IMAGE_TAG = "latest"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/Tarakananda/CI-CD.git'
      }
    }

    stage('Maven Build') {
      steps {
        sh './mvnw clean package -Pci'
      }
    }

    stage('Docker build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }

    stage('Docker compose up') {
      steps {
        sh 'docker compose down || true'
        sh 'docker compose up -d'
      }
    }

    stage('Health Check') {
      steps {
        sh '''
        sleep 20
        curl -f http://localhost:8081 || exit 1
        '''
      }
    }
  }

  post {
    success {
      echo 'Application Deployed SUCCESSFULLY'
    }
    failure {
      echo 'BUILD or Deployment FAILED'
    }
  }
}
