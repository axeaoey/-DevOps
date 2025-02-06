pipeline {
    agent any
    environment {
        DOCKER_SERVER = "43.208.253.87"
        DOCKER_PORT = "10080"
        IMAGE_NAME = "student-id-app"
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
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker tag $IMAGE_NAME $DOCKER_SERVER:$DOCKER_PORT/$IMAGE_NAME'
                sh 'docker push $DOCKER_SERVER:$DOCKER_PORT/$IMAGE_NAME'
            }
        }
        stage('Deploy on Docker Server') {
            steps {
                sshagent(['jenkins-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no user@$DOCKER_SERVER <<EOF
                    docker stop $IMAGE_NAME || true
                    docker rm $IMAGE_NAME || true
                    docker run -d -p 10080:80 --name $IMAGE_NAME $DOCKER_SERVER:$DOCKER_PORT/$IMAGE_NAME
                    EOF
                    '''
                }
            }
        }
    }
}
