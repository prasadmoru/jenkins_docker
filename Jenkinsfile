node{
 stage('Git Checkout'){
	git 'https://github.com/prasadmoru/jenkins_docker.git 
 }
 stage('Maven Package'){
	sh 'mvn clean package'
	sh 'mv target/myweb*.war target/myweb.war'
 }
 
 stage('Build Docker Imager'){
   sh 'docker build -t prasad890/jenkinscicd:0.0.1 .'
 }
 
 stage('Push to Docker Hub'){
 
	 withCredentials([string(credentialsId: 'github-pwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u prasad890 -p ${dockerHubPwd}"
     }
	 sh 'docker push prasad890/jenkinscicd:0.0.1'
 }
 stage('Remove Previous Container'){
	try{
		def dockerRm = 'docker rm -f myweb'
		sshagent(['docker-dev']) {
			sh "ssh -o StrictHostKeyChecking=no ubuntu@100.26.239.134 ${dockerRm}"
		}
	}catch(error){
		//  do nothing if there is an exception
	}
 }
 stage('Deploy to Dev Environment'){
   def dockerRun = 'docker run -d -p 8083:8080 --name myweb prasad890/jenkinscicd:0.0.1 '
   sshagent(['docker-dev']) {
    sh "ssh -o StrictHostKeyChecking=no ubuntu@35.153.83.160 ${dockerRun}"
   }

 }

}
