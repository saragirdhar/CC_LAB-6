pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker rmi -f backend-app || true'
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh 'docker rm -f backend1 backend2 || true'
                sh 'docker run -d --name backend1 backend-app'
                sh 'docker run -d --name backend2 backend-app'
                sh 'sleep 3'
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh 'docker rm -f nginx-lb || true'
                sh 'docker run -d --name nginx-lb -p 80:80 nginx'
                sh 'sleep 2'
                sh 'docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf'
                sh 'docker exec nginx-lb nginx -s reload'
            }
        }
    }
}
