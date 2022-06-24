#!/usr/bin/env groovy
pipeline {
    agent any
    tools {
            maven 'Maven 3.3.9'
            jdk 'jdk8'
    }
    stages {
        stage('Build & Test') {
            steps {
                 git 'https://github.com/Changolaxtra/cicd-demo'
                 sh 'mvn clean install'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}