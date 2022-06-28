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
        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build 'danrojas/spring-example'
                }
            }
        }
        stage('Docker Push'){
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}