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
        stage('Push image to DockerHub') {
            steps {
                withCredentials([usernamePassword(
			credentialsId: 'DockerHub',
			usernameVariable: 'dockeruser',
			passwordVariable: 'pass')])
		{
			sh '''
				docker login -u ${dockeruser} -p ${pass}
                        	docker image tag react-app:${BUILD_NUMBER} ${dockeruser}/react-app:${BUILD_NUMBER}
				docker image tag api-server:${BUILD_NUMBER} ${dockeruser}/api-server:${BUILD_NUMBER}
				docker push ${dockeruser}/react-app:${BUILD_NUMBER}
				docker push ${dockeruser}/api-server:${BUILD_NUMBER}
			'''	
		}
            }
        }
        stage('run the docker containers') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
