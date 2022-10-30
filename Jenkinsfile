pipeline {
    agent any
    environment {
        //be sure to replace "nagkagitha1" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "nagkagitha1/train-schedule"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World....!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB_LOGIN') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
		stage('Deploy-App-Prod') {
  	   steps {
              sh 'ansible-playbook --inventory /tmp/inv deploy-kube.yml --extra-vars "env=prod build=$BUILD_NUMBER"'
	   }
	   post { 
              always { 
                cleanWs() 
	      }
	   }
	}
    }
}
