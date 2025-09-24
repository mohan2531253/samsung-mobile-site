pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mohan2531253/samsung-mobile-site.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building project..."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mohan2366/samsung-site:v1 .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'mohan2366-dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push mohan2366/samsung-site:v1
                    '''
                }
            }
        }

        stage('Run Container (Local Test)') {
            steps {
                sh 'docker rm -f samsung-site-test || true'
                sh 'docker run -d -p 8081:80 --name samsung-site-test mohan2366/samsung-site:v1'
            }
        }

    }
}

