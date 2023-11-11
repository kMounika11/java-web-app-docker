node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/kMounika11/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
        stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.3", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t kmounika/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-hub', variable: 'docker-hub')]) {
          sh "docker login -u kmounika11 -p ${docker-hub}"
        }
        sh 'docker push kmounika11/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8083:8090 --name java-web-app kmounika11/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.23.19 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.23.19 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.23.19 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.23.19 ${dockerRun}"
       }
       
    }
     
     
}
