pipeline {
    agent any
    
    	environment {
		    PATH = "${PATH}:${getGradlePath()}"
		    DEV_SERVER_IP = "172.31.43.141"
    	}

    stages {
        stage("Build"){
            steps{
                sh 'gradle clean build'
            }
        }
        
        stage("Copy and Run"){
            steps {
                sshagent(['deploy-dev']) {
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@${DEV_SERVER_IP}'
                    sh 'scp  app/build/libs/*.jar ec2-user@${DEV_SERVER_IP}:~/'
                    sh 'ssh ec2-user@${DEV_SERVER_IP} java -jar app.jar'
                }
            }
        }
        
    }
}

def getGradlePath(){
	def Gradle = tool name: 'gradle7', type: 'gradle'
	return "${Gradle}/bin"
}
