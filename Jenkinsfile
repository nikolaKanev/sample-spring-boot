pipeline {

    agent {
        label 'slave'
    }

    tools {
        gradle 'gradle'
        dockerTool 'docker'
    }

    triggers {
        githubPush()
    }

    environment {
      DOCKERHUB_CREDENTIALS = credentials('docker')
      IMAGE_NAME = 'nikolaKan/slave'
    }

    stages {

        stage('Clean') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/nikolaKanev/sample-spring-boot.git'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew build' //This is for building the gredle project
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test' //This is for testing the gredle modules
            }
        }

	stage('Docker login'){
           steps {
               sh 'docker login -u $DOCKERHUB_CREDENTIALS  -p $DOCKERHUB_CREDENTIALS'
	   }
        }

        stage('Docker build and tag'){
            steps {
                sh 'docker build -t ${IMAGE_NAME} -f Dockerfile .'
                sh 'docker tag ${IMAGE_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

	stage('Docker Push'){
            steps {
              sh 'docker push ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Deploy') {
           steps {
                sh 'docker run -d -p 7000:8080 ${IMAGE_NAME}:${BUILD_NUMBER}'
           }
        }
    }
}