pipeline {
  agent any
  environment {
    IMAGE_NAME = "my-web-app:latest"
    CONTAINER_NAME = "my-web-app-container"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build Docker image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }
    stage('Stop old container') {
      steps {
        sh '''
          if docker ps -a --format '{{.Names}}' | grep -w $CONTAINER_NAME > /dev/null 2>&1; then
            echo "Stopping and removing existing container..."
            docker stop $CONTAINER_NAME || true
            docker rm $CONTAINER_NAME || true
          else
            echo "No existing container found."
          fi
        '''
      }
    }
    stage('Run container') {
      steps {
        sh 'docker run -d --name $CONTAINER_NAME -p 8080:80 $IMAGE_NAME'
      }
    }
  }
  post {
    success { echo "Deployment success. Visit port 8080." }
    failure { echo "Pipeline failed." }
  }
}
