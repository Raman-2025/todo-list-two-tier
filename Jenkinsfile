pipeline{
    agent {label "harry"}
    stages{
        stage('clone'){
            steps{
               git url: "https://github.com/Raman-2025/todo-list-two-tier.git", branch: "main"
               echo "code clone successful"
            }
        }
        stage('build'){
            steps{
                sh " docker build -t two-tier:latest . "
                echo "build image successful"
            }
        }
       stage('push to dockerhub'){
            steps{
        withCredentials([usernamePassword(
          credentialsId:"dockerHubCred",
          usernameVariable:"dockerHubUser",
          passwordVariable:"dockerHubPass"
        )]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
          sh "docker image tag two-tier  ${env.dockerHubUser}/two-tier:latest "
          sh "docker push  ${env.dockerHubUser}/two-tier:latest  "
                }
            }
        }  
        stage('deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d --build"
                echo "deploy successful"
            }
        }        
    }
}
