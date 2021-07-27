pipeline {
    agent any
    
    	environment {
		    PATH = "${PATH}:${getGradlePath()}"
		    DEV_IP = "172.31.43.141"
    	}

    stages {
        stage("scm checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/srinivasarao2468/gradle-project.git'
            }
        }  
      
      
        stage("Build"){
            steps{
                sh 'gradle clean build'
            }
        }
        
        stage("Copy File"){
            steps {
                sshagent(['deploy-dev']) {
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@${DEV_IP}'
                    sh 'scp  app/build/libs/*.jar ec2-user@${DEV_IP}:~/'
                    sh 'ssh ec2-user@${DEV_IP} java -jar app.jar'
                }
            }
        }
        
    }
}

def getGradlePath(){
	def Gradle = tool name: 'gradle7', type: 'gradle'
	return "${Gradle}/bin"
}
