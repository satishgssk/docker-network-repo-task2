pipeline {
    agent any
    environment {
        MYSQL_ROOT_PASSWORD=credentials("MYSQL_ROOT_PASSWORD")
    }
    stages {
        stage('Build') {
            steps {
               sh '''
                docker build -t satishgssk/task2-db db
                docker build -t satishgssk/task2-nginx nginx
                docker build -t satishgssk/task2-app flask-app
                '''
            }
        }
        stage('Push') {
            steps {
               sh '''
                docker push satishgssk/task2-nginx
                docker push satishgssk/task2-app
                docker push satishgssk/task2-db
                '''
            }
        }
        stage('Deploy') {
            steps { 
                sh '''
                ssh jenkins@satish-deploy <<EOF
                export MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
                docker network rm task2-net && echo "removed task2-net" || echo "already removed task2-net"
                docker network create task2-net
                docker stop task2-nginx && echo "stopped task2-nginx" || echo "task2-nginx is not running"
                docker rm task2-nginx && echo "removed task2-nginx" || echo "task2-nginx is not running"
                docker stop flask-app && echo "stopped flask-app" || echo "flask-app is not running"
                docker rm flask-app && echo "removed flask-app" || echo "flask-app is not running"
                docker stop mysql && echo "stopped mysql" || echo "mysql is not running"
                docker rm mysql && echo "removed mysql" || echo "mysql is not running"
                docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} --network task2-net satishgssk/task2-db
                docker run -d --name flask-app -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} --network task2-net satishgssk/task2-app
                docker run -d --name task1-nginx --network task2-net -p 80:80 satishgssk/task2-nginx
                '''
            }
        }
    }
}