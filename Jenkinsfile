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
            sh 'ssh automation@35.192.185.140 docker load  --input /tmp/jenkins.tar'
            sh 'docker images'
            sh 'docker run --name jenkins-jp  --rm --detach --env DOCKER_HOST=tcp://docker:2376 --publish 8080:8080 --volume jenkins-data:/var/jenkins_home jpjenkins'
        }
    }
}
    }
}
