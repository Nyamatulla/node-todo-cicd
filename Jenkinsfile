pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/Nyamatulla/node-todo-cicd.git", branch: "master"
                echo 'bhaiyya code clone ho gaya'
            }
        }
        stage('Build and Push Docker Image') {
  environment {
        DOCKER_IMAGE = "nyamatulla/todo-node-app:${BUILD_NUMBER}"
        
        REGISTRY_CREDENTIALS = credentials('dockerHub')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "dockerHub") {
                dockerImage.push()
            }
        }
      } 
  } 
        stage("scan image"){
            steps{
             echo 'image scanning ho gayi'
            }
        }
        

        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment ho gayi'
            }
        }
    }
}
