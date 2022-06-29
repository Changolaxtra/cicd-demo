#!/usr/bin/env groovy
pipeline {
    agent any
    environment {
            JAVA_HOME = '/opt/java/openjdk'
            registry = "darojas/spring-example"
    }
    tools {
            maven 'maven3'
            jdk 'jdk'
    }
    stages {
        stage('Build & Test') {
            steps {
                withEnv(["JAVA_HOME=/opt/java/openjdk"]){
                    sh 'echo "JAVA_HOME = ${JAVA_HOME}"'
                    sh 'echo "M2_HOME = ${M2_HOME}"'
                    git 'https://github.com/Changolaxtra/cicd-demo'
                    sh 'ls -la'
                    sh 'mvn clean install'
                }
            }
        }
        stage('Docker Build'){
            steps{
                script {
                    dockerImage = docker.build("darojas/spring-example")
                    dockerImage.tag("${env.BUILD_ID}")
                }
            }
        }
        stage('Docker Push'){
            steps {
                sh 'docker logout'
                script {
                    docker.withRegistry('', 'dockerId') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                sh 'cd deployment && ls -la'
                sh 'aws sts get-caller-identity'
                sh 'aws eks update-kubeconfig --name test-cluster --region us-west-2'
                sh 'aws eks get-token --cluster-name test-cluster'
                sh 'kubectl config view --minify'
                sh 'kubectl apply -f deployment.yml'
            }
        }
    }
}