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
                ansiblePlaybook credentialsId: 'private-key', installation: 'ansible', inventory: 'inv.yaml', playbook: 'play.yaml'
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

