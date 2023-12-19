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
        stage("push"){
            steps{
               // Retrieve Docker Hub credentials from Jenkins credentials store
                    withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                        
                        // Set environment variables
                        env.DOCKER_USERNAME = sh(script: 'echo \$dockerHubUser', returnStdout: true).trim()
                        env.DOCKER_PASSWORD = sh(script: 'echo \$dockerHubPass', returnStdout: true).trim()

                        // Log in to Docker Hub
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        
                        // Build and push Docker image
                        sh "docker tag node-app-test-new:latest ${DOCKER_USERNAME}/node-app-test-new:latest"
                        sh "docker push ${DOCKER_USERNAME}/node-app-test-new:latest"
                        echo 'image push ho gaya'
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
