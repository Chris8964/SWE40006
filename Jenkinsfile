pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                script {
                    git 'https://github.com/Chris8964/SWE40006.git'
                    checkout scm
                }
            }
        }
        
        stage('Install Composer Dependencies') {
            steps {
                sh 'composer install'
            }
        }
        
        stage('Run PHPStan tests') {
            steps {
                sh 'vendor/bin/phpstan analyse src/'
            }
        }

        stage('Merge Pull Request') {
            environment {
                BRANCH_TO_BE_MERGED = env.BRANCH_NAME
            }
            steps {
                sh 'git checkout master'

                sh "git merge ${BRANCH_TO_BE_MERGED}"

                sh 'git push -u origin master'
            }
        }
        /*
        stage('Install Prerequisites') {
            environment {
                AWS_SERVER = '54.206.74.246'
                AWS_USER = 'ubuntu'
                KEY_PATH = '/var/lib/jenkins/keys/jenkins.pem'
            }
            steps {
                script {
                    // Add Ondřej Surý PHP PPA repository
                    sshScript = """
                        ssh -i ${KEY_PATH} -o StrictHostKeyChecking=no ${AWS_USER}@${AWS_SERVER} 'sudo apt-get install -y software-properties-common && sudo add-apt-repository ppa:ondrej/php && sudo apt-get update'
                    """
                    sh "${sshScript}"

                    // Install PHP 8.2
                    sshScript = """
                        ssh -i ${KEY_PATH} ${AWS_USER}@${AWS_SERVER} 'sudo apt install -y php8.2'
                    """
                    sh "${sshScript}"

                    // Enable PHP module
                    sshScript = """
                        ssh -i ${KEY_PATH} ${AWS_USER}@${AWS_SERVER} 'sudo a2enmod php8.2'
                    """
                    sh "${sshScript}"
                }
            }
        } */
        stage('Deploy to AWS') {
            environment {
                AWS_SERVER = '54.206.74.246'
                AWS_USER = 'ubuntu'
                REMOTE_PATH = '/var/www/html/SWE40006'
                KEY_PATH = '/var/lib/jenkins/keys/jenkins.pem'
            }
            steps {
                script {
                    // Copy files to the remote server
                    sh "scp -i ${KEY_PATH} -r * ${AWS_USER}@${AWS_SERVER}:${REMOTE_PATH}"
                }
            }
        }
        stage('Start Server') {
            environment {
                AWS_SERVER = '54.206.74.246'
                AWS_USER = 'ubuntu'
                KEY_PATH = '/var/lib/jenkins/keys/jenkins.pem'
            }
            steps {
                script {
                    // Restart Apache
                    sshScript = """
                        ssh -i ${KEY_PATH} ${AWS_USER}@${AWS_SERVER} 'sudo systemctl restart apache2'
                    """
                    sh "${sshScript}"
                }
            }
        }
    }
}
