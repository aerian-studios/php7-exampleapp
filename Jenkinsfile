pipeline {
    agent {
      docker {
        image 'aerian-studios-jenkins/docker-build-image-ubuntu1804-php7:latest'
        args '--user=root'
      }
    }
    environment {
        REPO_NAME = "php7-exampleapp"
        SSH_PRIVATE_KEY_PATH = credentials('jenkins-ssh-key-file')
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'MINUTES')
    }
    stages {
        stage('Tool install') {
            steps {
                echo "Injecting SSH private key"
                sh 'mkdir ~/.ssh/ && cp $SSH_PRIVATE_KEY_PATH ~/.ssh/id_rsa'
                sh 'chmod 700 ~/.ssh/id_rsa'
                sh 'ssh -T -o StrictHostKeyChecking=no git@github.com || true'
                sh 'export PATH=/usr/bin:/usr/local/bin:$PATH'
            }
            post {
                success {
                    sh "echo Success"
                    sh "npm -v"
                    sh "nodejs -v"
                    sh "node -v"
                    sh "terraform -v"
                    sh "printenv"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'php -v'
                }
            }
        }
    }
}
