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
    stage('Deploy') {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
        sshagent (credentials: ['ssh-prod']) {
            sh 'mkdir -p ~/.ssh'
            sh 'ssh-keyscan -H "18.142.47.223" > ~/.ssh/known_hosts'
            sh "rsync -rav --delete ./laravel/ ubuntu@18.142.47.223:/home/ubuntu/prod.kelasdevops.xyz/ --exclude=.env --exclude=storage --exclude=.git"
            sh "ssh ubuntu@18.142.47.223 'cd ~/prod.kelasdevops.xyz/ && rm composer.lock && composer install'"
        }
    }
    }
}
