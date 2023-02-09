#!/usr/bin/env groovy
pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    environment {
        IMAGE_NAME = 'bilalasghar123/demo-app:java-maven-app-2.0'
    }
    stages {
        stage("build app") {
            steps {
                script {
                    echo "building the application..."
                    sh "mvn clean package"
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]){
                        sh "sudo docker build -t $IMAGE_NAME ."
                        sh "echo $PASS | sudo docker login -u $USER --password-stdin"
                        sh "sudo docker push $IMAGE_NAME"
                    }
                }
            }
        }
        stage("provision server") {
            environment {
                AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
                AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
            }
            steps {
                script {
                    dir('terraform') {
                        sh "terraform init"
                    }
                }
            }
        }
    }
}