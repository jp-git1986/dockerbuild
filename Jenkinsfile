pipeline {
    agent any

    stages {
        stage('gitcheckout') {
            steps {
                git branch: 'main', url: 'https://github.com/jp-git1986/dockerbuild.git'
            }
        }
        stage('dockerbuild') {
            steps {
                sh 'docker build -t jenkinsjp":$BUILD_NUMBER" .'
            }
        }
        stage('dockersave') {
            steps {
                sh 'docker save jenkinsjp":$BUILD_NUMBER" > jenkins.tar'
            }
        }
        stage ('copyandloadimage') {
    steps{
        sshagent(credentials : ['dockernode']) {
            sh 'scp -o StrictHostKeyChecking=no jenkins.tar automation@35.192.185.140:/tmp/'
            sh 'ssh automation@35.192.185.140 docker load </tmp/jenkins.tar'
            sh 'docker images'
        }
    }
}
    }
}
