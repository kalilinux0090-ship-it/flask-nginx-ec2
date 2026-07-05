pipeline {
    agent any

    environment {
        APP_NAME = "Flask-App"
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
