pipeline {
    agent any

    tools {
        maven "3.8.5"
    }

    stages {

        stage('Compile and Clean') {
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Deploy') {
            steps {
                sh "mvn package"
            }
        }

        stage('Build Docker image') {
            steps {
                echo "Hello Java Express"
                sh 'ls'
                sh 'docker build -t vaishnavibadhe/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
                    sh "docker login -u vaishnavibadhe -p ${Dockerpwd}"
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push vaishnavibadhe/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh 'docker run -itd -p 8085:8080 vaishnavibadhe/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        stage('Archiving') {
            steps {
                archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
