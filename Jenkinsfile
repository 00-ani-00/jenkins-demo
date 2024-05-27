pipeline {
    agent any 
    tools {
        docker 'docker'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('pull SCM'){
            steps{
            git branch: 'main', url: 'https://github.com/00-ani-00/jenkins-demo.git'
            }
        }
        stage('build,test and push'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){
                       sh 'docker build -t anilagad/Node-app .'
                       sh 'docker run -d --name my-cont -p 4000:3000 anilagad/Node-app'
                       sh 'docker push anilagad/Node-app'
                    }
                 }
             }
         }
      }
   }
