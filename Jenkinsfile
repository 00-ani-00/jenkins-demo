pipeline {
    agent any 
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        GIT_REPO_NAME = "jenkins-demo"
        GIT_USER_NAME = "00-ani-00"
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
                        sh 'docker tag node-app:${BUILD_NUMBER} anilagad/node-app:${BUILD_NUMBER}'
                        sh 'docker push anilagad/node-app:${BUILD_NUMBER}'
                    }
                }
            }
        }
        stage ('replce-docker-image'){
            steps {
                script{
                    withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        git config user.email "aniketlagad6@gmail.com"
                        git config user.name "00-ani-00"
                        sed -i '5 i\node-app: ${BUILD_NUMBER}/ ' test/docker-compose.yml
                        git add test/docker-compose.yml
                        git commit -m "Update docker-compose.yml file image to version ${BUILD_NUMBER}"
                        cat test/docker-compose.yml
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                        '''    
                    }
                }
            }
        }
        stage ('deploy application'){
            steps{
                sh '''
                docker stack deploy -c test/docker-compose.yml my-new-stack
                '''
            }
        }
    }
}
//sed -i "s/tag/${BUILD_NUMBER}/g" test/docker-compose.yml