node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/Soladipom/Spring-boot-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
     stage('deploy to ECR') {
       withAWS(credentials: 'moladipoawscred', region: 'us-east-1'){
           echo 'deploying docker image to aws ecr...'
           sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 516207934632.dkr.ecr.us-east-1.amazonaws.com'
           sh 'docker build -t springbootapp .'
           sh 'docker tag springbootapp:latest 516207934632.dkr.ecr.us-east-1.amazonaws.com/springbootapp:latest'
           sh 'docker push 516207934632.dkr.ecr.us-east-1.amazonaws.com/springbootapp:latest'
       }
     }
     stage('deploy to k8s cluster') {
       withAWS(credentials: 'moladipoawscred', region: 'us-west-1'){
       echo 'deployment into kubernetes cluster'
       sh 'kubectl apply -f springapp.yml'
        }
    }
     
}
