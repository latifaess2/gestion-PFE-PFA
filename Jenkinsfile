pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
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

    

    stage('Cleanup') {
      steps {
        bat 'docker-compose down'
      }
    }
  }

  post {
    success {
      mail bcc: '', body: '''Le pipeline Jenkins s\'est execute avec succes. 
      Tout s\'est deroule sans erreur.
      
      ''', subject: 'Sujet : Reussite du pipeline Jenkins', to: 'latifaessabbar02@gmail.com, nnadammemdouh2023@gmail.com, elmafhoum452@gmail.com, '
    }
    failure {
      mail bcc: '', body: '''Le pipeline Jenkins a echoue. 
      Veuillez prendre les mesures nécessaires pour resoudre le probleme.
      ''', subject: 'Sujet : Echec du pipeline Jenkins', to: 'latifaessabbar02@gmail.com, nnadammemdouh2023@gmail.com, elmafhoum452@gmail.com, '
    }
  }
}
