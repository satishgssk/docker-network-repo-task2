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
                sed -e 's, {{password}}, ${MYSQL_ROOT_PASSWORD}, g;' db-secret.yaml | kubectl apply -f -
                kubectl apply -f task2-db-manifest.yaml
                kubectl apply -f task2-app-manifest.yaml
                kubectl apply -f task2-nginx-manifest.yaml
                sleep 60
                kubectl get services
                '''
            }
        }
    }
}