pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backends') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true

                docker run -d --name backend1 --network lab6-net backend-app
                docker run -d --name backend2 --network lab6-net backend-app
                '''
            }
        }

        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-lb || true

                docker run -d --name nginx-lb --network lab6-net -p 80:80 \
                  -v /var/jenkins_home/workspace/LAB6-PIPELINE-NGINX/nginx/default.conf:/etc/nginx/conf.d/default.conf:ro \
                  nginx
                '''
            }
        }

        stage('Post Actions') {
            steps {
                echo 'Pipeline executed successfully. NGINX load balancer is running.'
            }
        }
    }
}
