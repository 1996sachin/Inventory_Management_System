pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/Inventory_Management_System'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/1996sachin/Inventory_Management_System.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
            }
        }

        stage('Deploy') {
            steps {
                sh """
                rsync -av --delete ./ $DEPLOY_DIR/
                cd $DEPLOY_DIR
                php artisan migrate --force
                php artisan config:cache
                php artisan route:cache
                php artisan view:cache
                chown -R www-data:www-data $DEPLOY_DIR
                chmod -R 755 $DEPLOY_DIR
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
