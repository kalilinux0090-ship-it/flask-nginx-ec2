pipeline {
    agent any

    environment {
        APP_NAME = "Flask-App"
        IMAGE_NAME = "raj01docker/flask-nginx-app"
    }

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'test', 'prod'],
            description: 'Select Deployment Environment'
        )
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kalilinux0090-ship-it/flask-nginx-ec2.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${APP_NAME} in ${params.ENV}"
            }
        }


        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Hub Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

       stage('Deploy') {
          steps {
             sh '''
             docker pull raj01docker/flask-nginx-app:latest
             docker stop flask-container || true
             docker rm flask-container || true
             docker run -d --name flask-container -p 5000:5000 raj01docker/flask-nginx-app:latest
             '''
          }
      }

    }


    post {
        always {
            echo "Pipeline Finished"
        }

        success {
            echo "Build Successful"
        }

        failure {
            echo "Build Failed"
        }
    }
}
