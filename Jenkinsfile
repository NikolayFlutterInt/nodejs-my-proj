pipeline {

    agent {
        label 'my-agent-1'
    }

    tools {
        nodejs 'NodeJS 20.2.0'
        dockerTool 'docker'
    }
    triggers {
        githubPush()
    }

    environment {
      DOCKERHUB_CREDENTIALS = credentials('DockerHub')
      IMAGE_NAME = 'ndimovflutter/mynodejsapp'
    }

    stages {
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        stage('CloneRepo') {
            steps {
                git branch: 'main', url: 'https://github.com/NikolayFlutterInt/nodejs-my-proj.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm ci' //This is for building the nodejs project
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' //This is for testing the nodejs modules
            }
        }
        stage('DockerLogin') {
            steps {
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
            }
        }
        stage('DockerBuildAndTag') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} -f Dockerfile .'
                sh 'docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }
        stage('DockerPush') {
            steps {
                sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker container rm -f mynodejsapp || true'
                sh 'docker container run -d --name mynodejsapp ndimovflutter/mynodejsapp'
            }
        }
    }
 }

//         stage('Deploy') {
//            steps {
//                 sh 'pkill node | true'
//                 sh 'npm install -g forever'
//                 sh 'forever start src/index.js'
//            }
//         }
//     }
// }
