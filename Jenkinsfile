pipeline {
 agent any

 environment {
  DOCKERHUB_CREDENTIALS=credentials('Docker-Hub')
 }

 stages {
  stage('Checkout portalhr from Github') {
   steps {
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/xsantq/hrportal-bare-metal-k8s.git'
   }
  }

  stage('maven install') {
      steps {
         withMaven(maven: 'Maven3') {
              sh 'mvn clean install'
        
            }
      }
    }
  stage('Login Docker Hub') {
   steps {
    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
   }
  }

  stage('Build docker image') {
   steps {
    sh 'docker build --tag=xsantq/hrportal:latest .'
    sh 'docker push xsantq/hrportal:latest'
   }
  }
 }

 post {
  always {
   sh 'docker logout'
  }
 }
}
