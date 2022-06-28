#!/usr/bin/env groovy
pipeline {
    agent any
    environment {
            JAVA_HOME = '/opt/java/openjdk'
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
        stage('Docker build') {
            steps {
                sh 'curl -sSL -O https://download.docker.com/linux/static/stable/x86_64/docker-20.10.9.tgz'
                sh 'mkdir tmp && tar zxf docker-20.10.9.tgz -C tmp'
                sh 'ls -la tmp && ls -la tmp/docker'
                sh 'mv ./tmp/docker/docker ./docker && chmod +x ./docker'
                sh './docker build -t danrojas/spring-example:latest .'
            }
        }
         stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "./docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh './docker push danrojas/spring-example:latest'
                }
            }
         }
    }
}