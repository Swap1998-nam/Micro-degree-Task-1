pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-nginx"
        CONTAINER_NAME = "nginx-container"
        HOST_PORT = "9010"
        CONTAINER_PORT = "80"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop and remove running OR stopped container with the same name
                    sh '''
                    if [ "$(docker ps -aq -f name=$CONTAINER_NAME)" ]; then
                        docker rm -f $CONTAINER_NAME
                    fi
                    docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                script {
                    sh 'curl -I http://localhost:$HOST_PORT || true'
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
    }
}
