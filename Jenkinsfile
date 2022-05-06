pipeline{
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh "echo staring build the image"
                sh 'docker build -t viraj5132/canary:latest .'
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh "echo staring deploy the image"
                sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                sh 'docker push viraj5132/canary:latest'
            }
        }
        stage('Remove Docker Image') {
            steps {
                   sh "echo staring deploy the image"
            //       sh "docker rmi -f viraj5132/nodejsapp-1.0"  
            //       sh "ssh ubuntu@$DEPLOY_IP kubectl delete deploy nodejs-app"  
            }
        }
        stage('Deploy to Kubernetes in stage') {
              when { branch 'prod'}
            steps {
                sshagent(['3.91.222.148']) {
                    sh "echo staring deploy the image in Kubernetes"
                    sh "scp -o StrictHostKeyChecking=no stage.yaml ubuntu@$DEPLOY_IP:/home/ubuntu/"
                    sh "ssh ubuntu@$DEPLOY_IP kubectl apply -f stage.yaml" 
                }
            }
        }
         stage('Deploy to Kubernetes in prod') {
              when { branch 'stage'}
            steps {
                sshagent(['3.91.222.148']) {
                    sh "echo staring deploy the image in Kubernetes"
                    sh "scp -o StrictHostKeyChecking=no prod.yaml  ubuntu@$DEPLOY_IP:/home/ubuntu/"
                    sh "ssh ubuntu@$DEPLOY_IP kubectl apply -f prod.yaml" 
                }
            }
        }
   
    }
}