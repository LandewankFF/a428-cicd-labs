pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 4000:4000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    currentBuild.resultIsEqualTo("SUCCESS") ?: error("Pipeline has failed. Cannot proceed.")
                }
                input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', fail: 'Abort'
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
