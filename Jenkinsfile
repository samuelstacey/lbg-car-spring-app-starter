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
        stage ('Build Backend'){
            steps{
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage('Test and build react frontend') {
            steps {
                dir("lbg-car-front") {
                    sh """
                    npm install -g yarn
                    yarn install
                    yarn test
                    docker build -t sam473/lbg-car-front:v${BUILD_NUMBER} .
                    """
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
        stage ('Deploy apps to Server'){
            steps{
               sh ' echo "will do this"'
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