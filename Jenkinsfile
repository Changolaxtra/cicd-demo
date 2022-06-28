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
                    sh 'mvn clean install'
                }
            }
        }
        stage('Docker Build'){
            steps{
                script {
                    dockerImage = docker.build("spring-example")
                    dockerImage.tag("darojas/spring-example:${env.BUILD_ID}")
                }
            }
        }
        stage('Docker Push'){
            steps {
                sh 'docker logout'
                script {
                    docker.withRegistry('https://docker.io', 'dockerId') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}