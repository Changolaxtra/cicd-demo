#!/usr/bin/env groovy
pipeline {
    agent any
    environment {
            JAVA_HOME = '/opt/java/openjdk'
    }
    tools {
            maven 'maven3'
            jdk 'jdk'
            dockerTool 'docker'
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
        stage('Docker build & push') {
            steps {
                docker.withRegistry('https://hub.docker.com/', 'dockerHub') {
                  docker.build('danrojas/spring-example').push('latest')
                }
            }
        }
    }
}