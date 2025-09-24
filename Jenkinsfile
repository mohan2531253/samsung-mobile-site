pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url:https://github.com/mohan2531253/samsung-mobile-site.git
            }
        } 
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }


        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t mohan2366/samsung-site:v1 .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mohan2366-dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push mohan2366/samsung-site:v1"
                }
            }
        }

        stage('Run Container (Local Test)') {
            steps {
                sh 'docker run -d -p 8081:80 --name samsung-site-test mohan2366/samsung-site:v1 || true'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                docker service rm samsung-site || true
                docker service create --name samsung-site -p 8081:80 mohan2366/samsung-site:v1
                '''
            }
        }
    }
}


