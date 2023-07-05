node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
    
    stage('Clone Repo') {
      git 'https://github.com/malikhammami/Jenkins-Test.git'
    }    
  
    stage('Build Project') {
      sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }
    
    stage('Initialize Docker'){         
	  def dockerHome = tool 'MyDocker'         
	  env.PATH = "${dockerHome}/bin:${env.PATH}"     
    }
    
    stage('Build Docker Image') {
      sh "docker -H tcp://192.168.22.136:4243 build -t devopsexample:${env.BUILD_NUMBER} ."
    }
	stage('Delete Old docker container'){
	sh "docker -H tcp://192.168.22.136:4243 rm -f  devopsexample"
    }
    
    stage('Deploy Docker Image'){
      	echo "Docker Image Tag Name: ${dockerImageTag}"
	sh "docker -H tcp://192.168.22.136:4243 run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"
    }

}
