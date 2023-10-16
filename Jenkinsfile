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
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input message: 'Lanjutkan ke tahap Deploy?', parameters: [choice(name: 'Pilihan', choices: 'Proceed\nAbort', description: 'Pilih untuk melanjutkan atau menghentikan')]
                    if (userInput == 'Abort') {
                        error('Eksekusi pipeline dihentikan oleh pengguna.')
                    }
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh 'sleep 60'
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}
