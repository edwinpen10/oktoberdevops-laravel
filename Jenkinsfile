node {
    checkout scm

    stage('Build') {
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'rm composer.lock'
            sh 'composer install'
        }
    }
    stage('Test') {
        docker.image('php:7.4-cli').inside('-u root') {
            sh 'php artisan test --testsuite=Unit'
        }
    }
 

   
}
