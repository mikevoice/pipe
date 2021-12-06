pipeline {
    agent {
        label 'master'
    } 
    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/mikevoice/project.git',
                branch: 'main',
                credentialsId: "token_g"
            }
        }
        stage('Jenkins play ansible') {
            steps {
                sh """#!/bin/bash
                ssh ansible@192.168.0.165 ansible --version
                echo "======Hello====="
                ssh ansible@192.168.0.165 ansible -m ping all
                who
                date
                git status
                """
            }
        }
    }    
    post {
        success {
                slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
            failure {
                slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        always {
            deleteDir()
        }
    }
}

