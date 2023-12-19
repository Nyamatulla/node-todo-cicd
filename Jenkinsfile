pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/Nyamatulla/node-todo-cicd.git", branch: "master"
                echo 'bhaiyya code clone ho gaya'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code build bhi ho gaya'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning ho gayi'
            }
        }
        
        stage('Push') {
            environment {
                DOCKER_USERNAME = ""
                DOCKER_PASSWORD = ""
            }
            steps {
                script {
                    // Retrieve Docker Hub credentials from Jenkins credentials store
                    withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                        
                        // Set environment variables
                        DOCKER_USERNAME = sh(script: 'echo \${DOCKERHUB_USER}', returnStdout: true).trim()
                        DOCKER_PASSWORD = sh(script: 'echo \${DOCKERHUB_PASS}', returnStdout: true).trim()

                        // Log in to Docker Hub
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        
                        // Build and push Docker image
                        sh "docker tag node-app-test-new:latest ${DOCKER_USERNAME}/node-app-test-new:latest"
                        sh "docker push ${DOCKER_USERNAME}/node-app-test-new:latest"
                    }
                }
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
