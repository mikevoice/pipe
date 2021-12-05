pipeline {
    agent {
        kubernetes {
        label 'promo-app'  // all your pods will be named with this prefix, followed by a unique id
        idleMinutes 5  // how long the pod will live after no jobs have run on it
        yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
        defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
        }
    }
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

