pipeline {
    agent any
    environment {
        registry = "vennilavan/demodev"
        AWS_REGION = 'us-east-2'
        EKS_CLUSTER_NAME = 'my-k8s-cluster'
        KUBE_NAMESPACE = 'default'
         
    }     
   
    stages {
    // Building Docker images
        stage('Building image') {
            steps{
                script {
                dockerImage = docker.build registry 
                }
            }
        }
   
    // Uploading Docker images into AWS ECR
        stage('Pushing to Dockerhub') {
            steps{  
                script { 
                        sh "docker login --username vennilavan -p $Pass"
                        sh 'docker push vennilavan/demodev'
                    }
                            
         }
        }
      }

       stage('K8S Deploy') {
         steps{
            script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS']]) {
               withKubeConfig(credentialsId: 'newk8s', serverUrl: 'https://8D2BD626D7B16FD912608A73CA98BEF2.gr7.us-east-2.eks.amazonaws.com') {
                  sh 'kubectl apply -f deploy.yaml'
                  }
               }
               
               }
            }        
         }
    }
}
