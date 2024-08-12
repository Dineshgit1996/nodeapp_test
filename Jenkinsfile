pipeline {
    agent any
    tools{
        nodejs 'node20'
    }

    stages {
        stage('git clone') {
            steps {
                git 'https://github.com/Dineshgit1996/nodeapp_test.git'
            }
        }
        
            stage('Installing Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
            stage('Image Creation and Tagging') {
            steps {
              sh 'docker build -t dineshraju1186/nodeapp:1.$BUILD_NUMBER .'
            }
        }
        
            stage('Pushing to DockerHub') {
            steps {
                script{
                withDockerRegistry(credentialsId: 'dockercredential', toolName: 'docker', url: ' https://index.docker.io/v1/') {
                 sh 'docker push dineshraju1186/nodeapp:1.$BUILD_NUMBER '
                }
              }
            }
        }
        
           stage('Deploying to k8s') {
            steps {
              script{
                  kubeconfig(credentialsId: 'config', serverUrl: '') {
                  sh 'cat deploymentservice.yml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
                  sh 'sleep 60'
                //   sh 'kubectl apply -f deploymentservice.yml'
                  sh 'kubectl get all -o wide'
                 }
              }
            }
        } 
        
    }
}
