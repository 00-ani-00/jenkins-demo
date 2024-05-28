pipeline {
    agent any 
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
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
                        sh 'docker build -t node-app:${BUILD_NUMBER} test/ '
                        sh 'docker tag Node-app:${BUILD_NUMBER} anilagad/node-app:${BUILD_NUMBER}'
                        sh 'docker run -d --name my-cont -p 4000:3000 anilagad/Node-app:${BUILD_NUMBER}'
                        sh 'docker push anilagad/Node-app:${BUILD_NUMBER}'
                    }
                }
            }
        }
    }
}
