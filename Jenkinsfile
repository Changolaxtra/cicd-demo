#!/usr/bin/env groovy
pipeline {
    agent any

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