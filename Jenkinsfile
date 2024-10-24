groovy
   pipeline {
       agent any

       environment {
           DOCKER_HUB_REPO = 'gremedio/guido-docker-image'
       }

       stages {
           stage('Clone repository') {
               steps {
                   git 'https://github.com/gremedio/guidorepo.git'
               }
           }

           stage('Build Docker Image') {
               steps {
                   script {
                       dockerImage = docker.build("${DOCKER_HUB_REPO}:${env.BUILD_ID}")
                   }
               }
           }

           stage('Run Tests') {
               steps {
                   script {
                       dockerImage.inside {
                           sh 'make test'  // Sostituisci con il comando per i tuoi test
                       }
                   }
               }
           }

           stage('Push to DockerHub') {
               steps {
                   script {
                       docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                           dockerImage.push("${env.BUILD_ID}")
                           dockerImage.push("latest")
                       }
                   }
               }
           }
       }

       post {
           always {
               cleanWs() // Pulisce la workspace di Jenkins
           }
       }
   }
