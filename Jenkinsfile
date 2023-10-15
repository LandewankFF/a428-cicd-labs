pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deploy.sh'
                timeout(time: 1, unit: 'MINUTES') {
                    input message: 'Aplikasi telah di-deploy. Tunggu 1 menit sebelum otomatis berakhir.'
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
} 