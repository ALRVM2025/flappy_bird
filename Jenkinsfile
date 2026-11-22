pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Pygbag') {
            steps {
                sh '''
                    echo "Очищаем старую сборку..."
                    rm -rf build/
                    
                    echo "Собираем игру через Pygbag..."
                    python3 -m pygbag --build main.py
                '''
            }
        }

        stage('Deploy to Netlify') {
            steps {
                withCredentials([
                    string(credentialsId: 'netlify-token', variable: 'NETLIFY_AUTH_TOKEN'),
                    string(credentialsId: 'netlify-site-id', variable: 'NETLIFY_SITE_ID')
                ]) {
                    sh '''
                        echo "Деплой на Netlify..."
                        netlify deploy --prod --dir=build/web --site=$NETLIFY_SITE_ID --auth=$NETLIFY_AUTH_TOKEN
                    '''
                }
            }
        }
    }
}