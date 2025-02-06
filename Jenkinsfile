pipeline {
    agent any
    environment {
        DOCKER_SERVER = "43.208.253.87"
        IMAGE_NAME = "66026066-app"
        CONTAINER_NAME = "66026066-container"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/USERNAME/REPO_NAME.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
                sh 'docker tag $IMAGE_NAME $DOCKER_SERVER:10080/$IMAGE_NAME'
                sh 'docker push $DOCKER_SERVER:10080/$IMAGE_NAME'
            }
        }
        stage('Deploy on Docker Server') {
            steps {
                sshagent(['jenkins-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no user@$DOCKER_SERVER <<EOF
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d -p 10080:80 --name $CONTAINER_NAME $DOCKER_SERVER:10080/$IMAGE_NAME
                    EOF
                    '''
                }
            }
        }
    }
}
