node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/Soladipom/Spring-boot-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    stage("Docker Build & Push"){
      withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
      sh "docker build -t moladipos/mymain1 ."
      sh "docker tag moladipos/mymain1 moladipos/mymain1:5"
      sh "docker push moladipos/mymain1:5"
        }
    }
     
    stage('Deploy to kubernetes'){
      withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
      sh 'kubectl apply -f springapp.yml'
        }
    }
     
}
