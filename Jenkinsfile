pipeline {
    agent any
    
    stages{
        stage('Code'){
            steps {
                git url: 'https://github.com/Fatima-Ishrath021/node-todo-cicd.git', branch: 'master'
            }
        }
      
        stage('Build and Test'){
            steps {
                sh 'docker build . -t fatima021/node-todo-app-cicd:latest' 
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'dockerHub',passwordVariable:'dockerHubPassword', usernameVariable:'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push fatima021/node-todo-app-cicd:latest"
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image fatima021/node-todo-app-cicd:latest > trivyimage.txt" 
            }
        }
        stage('Deploy to container'){
            steps{
                sh "docker run -d --name todo-app -p 8000:8000 fatima021/node-todo-app-cicd:latest"
            }
        }
    }
}
