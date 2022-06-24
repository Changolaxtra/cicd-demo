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
     stage ('Initialize') {
                steps {
                    sh '''
                        echo "JAVA_HOME = ${JAVA_HOME}"
                        echo "M2_HOME = ${M2_HOME}"
                    '''
        }
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