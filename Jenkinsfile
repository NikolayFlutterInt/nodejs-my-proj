pipeline {
    agent {
        label 'docker-slave'
    }

    tools {
        nodejs 'NodeJS 20.2.0'
    }

    stages {
        stage('CloneRepo') {
            steps {
                git branch: 'main', url: 'https://github.com/NikolayFlutterInt/nodejs-my-proj.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm ci'  //This is for building the nodejs project
            }
        }
        stage('Test') {
            steps {
                sh 'npm test' //This is for testing the nodejs modules
             }
        }
    }
}
