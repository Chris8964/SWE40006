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

        /*stage('Merge Pull Request') {
            environment {
                BRANCH_TO_BE_MERGED = "origin/${env.BRANCH_NAME}"
                REPOSITORY_URL = "https://github.com/Chris8964/SWE40006"
            }
            steps {
                sh 'git checkout master'

                sh "git merge ${BRANCH_TO_BE_MERGED}"

                sh "git push ${REPOSITORY_URL}"
            }
        }*/
    }
}
