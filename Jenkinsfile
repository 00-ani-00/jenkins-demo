pipeline {
    agent any 
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/00-ani-00/jenkins-demo.git'
            }
        }
        stage('Build, Test, and Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh 'docker build -t anilagad/Node-app .'
                        sh 'docker run -d --name my-cont -p 4000:3000 anilagad/Node-app'
                        sh 'docker push anilagad/Node-app'
                    }
                }
            }
        }
    }
}
