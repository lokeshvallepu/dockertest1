pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone --branch master https://github.com/mavrick202/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline1/dockertest1'
            sh 'cp  /var/lib/jenkins/workspace/pipeline1/dockertest1/* /var/lib/jenkins/workspace/pipeline1'
            sh 'docker build -t lokeshvallepu/apple2:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push lokeshvallepu/apple2:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://52.66.248.179:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://52.66.248.179:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 lokeshvallepu/apple2:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://52.66.248.179:8000'
          }
        }

    }
}
