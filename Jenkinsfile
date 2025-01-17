pipeline {
    environment {
        imageName = "jjvdgeer/docker-registry-browser"
        registry = "http://qnap:5000/"
        dockerImage = ''
    }
    agent { label 'docker' }
    stages {
        stage('Clone sources') {
            steps {
                git url: 'https://github.com/jjvdgeer/docker-registry-browser.git'
            }
        }
        stage('Building our image') {
            steps {
                script {
                    docker.withRegistry("$registry") {
                        dockerImage = docker.build imageName + ":$BUILD_NUMBER"
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Tag as latest') {
            steps {
                script {
                    docker.withRegistry("$registry") {
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $imageName:$BUILD_NUMBER"
            }
        }
    }
}
