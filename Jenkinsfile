pipeline {
    agent {
        label 'slave'
    }

    triggers {
        githubPush ()
    }


    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/nikolaKanev/sample-spring-boot.git'
            }
        }
    }
}