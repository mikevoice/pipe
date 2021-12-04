pipeline {
    agent {
        kubernetes {
            containerTemplate {
            name 'kubeval-test-yaml'
            image 'garethr/kubeval:0.15.0'
            ttyEnabled true
            command 'watch date'
            }
        }
    }
    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com:mikevoice/pipe.git',
                branch: 'master',
                credentialsId: "token_g"
            }
        }
        stage('Kubeval test') {
            steps {
                sh """
                kubeval deploy.yaml -o json
                kubeval ingres.yaml -o json
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                kubectl apply -f deploy.yaml
                kubectl apply -f ingres.yaml
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

