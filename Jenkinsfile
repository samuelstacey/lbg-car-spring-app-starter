pipeline {
    environment {
       registry = "sam473/cardatabase"
              registryCredentials = "dockerhub_id"
              dockerImage = ""
       }
    agent any
    stages {
        stage('Checkout source repos') {
            steps {
                dir("lbg-car-front") {
                    git url: "https://github.com/QA-Instructor/lbg-car-react-completed.git", branch: "main"
                }
                dir("lbg-car-back") {
                    git url: "https://github.com/QA-Instructor/lbg-car-spring-completed.git", branch: "main"
                }
            }
        }
        stage ('Build Docker Image'){
            steps{
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage ("Push to Docker Hub"){
            steps {
                script {
                    docker.withRegistry('', registryCredentials) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage ("Clean up"){
            steps {
                script {
                    sh 'docker image prune --all --force --filter "until=48h"'
                       }
            }
        }
    }
}