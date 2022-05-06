pipeline{
    agent any
    stages {

          stage('Deploy to Kubernetes in stage') {
              when { branch 'stage'}
            steps {
                sshagent(['3.91.222.148']) {
                    sh "echo staring deploy the image in Kubernetes"
                    // sh "scp -o StrictHostKeyChecking=no stage.yaml ubuntu@$DEPLOY_IP:/home/ubuntu/"
                    sh "ssh ubuntu@$DEPLOY_IP kubectl rollout restart deployment stage " 
                }
            }
        }
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
         stage('Deploy to Kubernetes in prod') {
              when { branch 'stage'}
            steps {
                sshagent(['3.91.222.148']) {
                    sh "echo staring deploy the image in Kubernetes"
                    sh "scp -o StrictHostKeyChecking=no prod.yaml  ubuntu@$DEPLOY_IP:/home/ubuntu/"
                    sh "ssh ubuntu@$DEPLOY_IP kubectl rollout restart deployment prod " 
                }
            }
        }
   
    }
}