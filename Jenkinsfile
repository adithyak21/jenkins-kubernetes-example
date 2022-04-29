pipeline{
    agent any
    stages {
        stage('Build Maven') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/adithyak21/jenkins-kubernetes-example.git']]])

             
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t adithyak12/nodejsapp-1.0:latest .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u adithyak21 -p ${dockerhubpwd}'
                 }  
                 sh 'docker push devopshint/nodejsapp-1.0:latest'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
            sshagent(['k8s']) {
            sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@IPofk8scluster:/home/ubuntu"
            script {
                try{
                    sh "ssh ubuntu@IPofk8scluster kubectl create -f ."
                }catch(error){
                    sh "ssh ubuntu@IPofk8scluster kubectl create -f ."
            }
}
        }
      
    }
    }
    }
}
