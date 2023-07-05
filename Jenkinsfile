node {
  def mvnHome = tool 'maven-3.5.2'
  def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
  def deploymentName = "devopsexample-deployment${env.BUILD_NUMBER}"
  
  stage('Clone Repo') {
    git 'https://github.com/malikhammami/Jenkins-Test.git'
  }    
  
  stage('Build Project') {
    sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
  }
  
  stage('Build Docker Image') {
    sh "docker build -t devopsexample:${dockerImageTag} ."
  }
  
  stage('Push Docker Image to Registry') {
    sh "docker push devopsexample:${dockerImageTag}"
  }
  
  stage('Initialize Kubernetes') {
    sh 'kubectl config use-context my-kubernetes-cluster'
  }
  
  stage('Create Kubernetes Deployment') {
    sh "kubectl create deployment ${deploymentName} --image=devopsexample:${dockerImageTag}"
  }
  
  stage('Expose Deployment as Service') {
    sh "kubectl expose deployment/${deploymentName} --port=2222 --target-port=2222 --type=LoadBalancer"
  }
}
