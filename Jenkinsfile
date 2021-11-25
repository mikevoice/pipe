peline {
    agent {
       node {
         label "Mike_test"
       }
    }
    triggers {
       cron('H 1 * * 1-5')
    }
    stages {
       stage('nmap') {
          steps {
             sh "nmap -sn 192.168.0.1/24 > scan_ip.txt"
          }
       }
       stage('speedtest') {
          steps {
             sh "speedtest > speedtest.txt"
          }
       }
       stage('clone repository github') {
          steps {
             git url: 'https://github.com/mikevoice/pipe.git', branch: 'master', credentialsId: "node_to_git"
          }
       }
       stage('push github') {
          steps {
             sh """
             git add --all
             git commit -m "add scan_ip.txt and speedtest.txt"
             git push -u origin master
             """
          }
       }
    }
}

