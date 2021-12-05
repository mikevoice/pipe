pipeline {
    agent none
    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/mikevoice/project.git',
                branch: 'main',
                credentialsId: "token_g"
            }
        }
        stage('Kubeval test') {
            steps {
                sh """
                ip addr
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                kubectl get ns
                # kubectl apply -f deploy.yaml
                # kubectl apply -f ingress.yaml
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

