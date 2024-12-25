pipeline{
    agent {label "agent"}
    stages{
        stage("git-clone"){
            steps{
                echo "git-clone done"
                git 'https://github.com/LondheShubham153/node-todo-cicd.git'
            }
        }
        stage("docker build n test"){
            steps{
                sh "docker build -t node-app ."
            }
        }
        stage("docker-push"){
            steps{
               withCredentials([usernamePassword(credentialsId: "dockerhubcreds", 
                                          usernameVariable: "dockerhubuser", 
                                          passwordVariable: "dockerhubpass")]) {
            sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
            sh "docker image tag node-app:latest ${env.dockerhubuser}/node-app:latest"
            sh "docker push ${env.dockerhubuser}/node-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
