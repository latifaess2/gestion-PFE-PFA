pipeline {

  agent any


  environment {
    DOCKERHUB_CREDENTIALS = credentials ('latifa-dockerhub')
    DOCKERHUB_REPO = 'gestiondesprojets'               // Replace with your Docker Hub repository
    DOCKERHUB_IMAGE = 'image_web' 
  }
  stages {
  stage('git clone') {
    steps {
      git 'https://github.com/latifaess2/gestion-PFE-PFA.git'
    } 
  }


  stage('Build') {
    steps {
      bat 'docker-compose build'
      
    }
  }

  stage('login to Docker Hub') {
    steps {
      script {
                    withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Login to Docker Hub
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    }
                }
    }
  }
  stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('', env.DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${env.DOCKERHUB_REPO}/${env.DOCKERHUB_IMAGE}").push()
                    }
                }
            }
        }
  stage('Deploy') {
            steps {
                // Deploy your application, e.g., using docker-compose
                sh 'docker-compose up -d'
            }
        }
    }

  


  stage('Cleanup') {
    steps {
      bat 'docker-compose down'
    }
  }
}

post {
        always {
            // Clean up Docker environment
            sh 'docker logout'
        }
    }
