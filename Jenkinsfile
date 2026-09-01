pipeline {
    agent {label 'dev'}

    stages {
        stage('clone from git hub') {
            steps {
                git url: 'https://github.com/joelpaul18/mern_docker_jenkins.git', branch: 'main'
            }
        }
        stage('build image') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('run the docker containers') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
