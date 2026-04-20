pipeline {
    agent any

    tools {
        nodejs 'NodeJS22'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    rm -rf /var/www/standupindojaksel/*
                    cp -r dist/* /var/www/standupindojaksel/
                    echo "Deploy selesai: $(date)"
                '''
            }
        }
    }

    post {
        success {
            echo 'Build dan deploy berhasil!'
        }
        failure {
            echo 'Build atau deploy gagal. Cek log di atas.'
        }
    }
}
