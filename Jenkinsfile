pipeline {
    agent {
        node {
            label "Mike_test"
        }
    }
    stages {
        stage('Clone repository') {
            steps {
                git url: 'git@github.com:mikevoice/pipe.git',
                branch: 'master',
                credentialsId: "node_to_git"
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

