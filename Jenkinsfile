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
                docker build -t jenkinsjp:1.0 .
            }
        }
        stage('dockersave') {
            steps {
                sh 'docker save jenkinsjp:1.0 > jenkins.tar'
            }
        }
        stage ('copyandloadimage') {
    steps{
        sshagent(credentials : ['dockernode']) {
            sh 'scp jenkins.tar automation@35.192.185.140:/tmp/'
            sh 'docker load >/tmp/jenkins.tar'
            sh 'docker images'
        }
    }
}
    }
}
