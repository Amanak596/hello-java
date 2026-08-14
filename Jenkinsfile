pipeline {
    agent any

    environment {
        IMAGE_NAME = "aman5250/hello-java"
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
    steps {
        sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
    }
}

stage('Docker Push') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin
                docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                docker logout
            '''
              }
         }
      }
           stage('Docker Deploy') {
    steps {
        sh '''
            docker pull ${IMAGE_NAME}:${BUILD_NUMBER}
            docker rm -f hello-java || true
            docker run -d --name hello-java ${IMAGE_NAME}:${BUILD_NUMBER}
        '''
    }
}
    }
}
