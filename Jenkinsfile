node{
   stage('SCM Checkout'){
     git 'https://github.com/damodaranj/my-app.git'
   }
   stage('Maven stage'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
    }
    stage('Remove Previous Image'){
        sh 'docker rmi -f 606a15f6933b f66b7f5ac535 2554d8c5a112 cc0f411710e8 cdd517ede416'
    }
    stage('Build Docker Imager'){
      sh 'docker build -t dkdeepak/myweb:1.0.2 .'
}
    stage('Docker Image Push'){
         withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
      sh "docker login -u dkdeepak -p ${dockerPassword}"
    }
   sh 'docker push dkdeepak/myweb:1.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin1234 13.233.138.84:8082"
   sh "docker tag dkdeepak/myweb:1.0.2 13.233.138.84:8082/app:1.0.0"
   sh 'docker push 13.233.138.84:8082/app:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest dkdeepak/myweb:1.0.2' 
}
}
}
