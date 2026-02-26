pipeline {
    agent any

    stages {
        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Create Network') {
            steps {
                sh 'docker network create lab-net || true'
            }
        }

        stage('Run Backend Containers') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true
                docker run -d --name backend1 --network lab-net backend-app
                docker run -d --name backend2 --network lab-net backend-app
                sleep 5
                docker ps
                '''
            }
        }

        stage('Run NGINX Load Balancer') {
    steps {
        sh '''
        docker rm -f nginx-lb || true
        docker run -d -p 80:80 --name nginx-lb --network lab-net nginx:latest
        docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
        docker restart nginx-lb
        '''
    }
}
    }
}
