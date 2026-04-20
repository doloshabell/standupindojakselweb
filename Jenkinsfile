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
                withCredentials([
                    string(credentialsId: 'STANDUPINDOJAKSEL_PUBLIC_APPS_SCRIPT_URL', variable: 'PUBLIC_APPS_SCRIPT_URL'),
                    string(credentialsId: 'STANDUPINDOJAKSEL_PUBLIC_FORM_SECRET', variable: 'PUBLIC_FORM_SECRET')
                ]) {
                    sh '''
                        echo "PUBLIC_APPS_SCRIPT_URL=$PUBLIC_APPS_SCRIPT_URL" > .env
                        echo "PUBLIC_FORM_SECRET=$PUBLIC_FORM_SECRET" >> .env
                        npm run build
                    '''
                }
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
