pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('latifa_dockerhub')
  }
  
  stages {
    stage('Checkout code') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        bat 'docker-compose build'
        bat 'start docker-compose up -d'
      }
    }

    stage('Test') {
      steps {
        echo "test passed"
      }
    }

    stage('Push Images to Docker Hub') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'latifa_dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
            bat 'echo %DOCKER_PASSWORD%| docker login -u %DOCKER_USERNAME% --password-stdin'
          }
        }
        bat 'docker-compose push'
      }
    }

    stage('Cleanup') {
      steps {
        bat 'docker-compose down'
      }
    }
  }

  post {
    success {
      mail bcc: '', body: '''
      Le pipeline Jenkins s'est exécuté avec succès. 
      Tout s'est déroulé sans erreur.
      ''', subject: 'Sujet : Réussite du pipeline Jenkins', to: 'latifaessabbar02@gmail.com'
    }
    failure {
      mail bcc: '', body: '''
      Le pipeline Jenkins a échoué. 
      Veuillez prendre les mesures nécessaires pour résoudre le problème.
      ''', subject: 'Sujet : Échec du pipeline Jenkins', to: 'latifaessabbar02@gmail.com'
    }
  }
}
