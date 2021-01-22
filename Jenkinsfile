pipeline {
    agent any
    environment {
        containerName = ""
        container_version = "1.0.0.${BUILD_ID}"
        dockerTag = "${containerName}:${container_version}"
	registry = "shraddhal/tomcatserver-jenkinsanddocker-local"
    	registryCredential ='76599700-71c5-4af4-b805-1bcd97a088e4'
    }
    stages{
        
        stage('Clone repository') {
			   steps {	       
				 checkout scm }
	}
        
       stage('Build') {
			   steps {	       
				/*  bat 'rm target/roshambo.war'
                 		bat  'rm -rf target/roshambo' */
               			bat 'mvn clean package'
               } 
        
	}
        
        
        stage ('Build Container') {
            steps {
		    dockerImage  = docker.build registry + ":$BUILD_NUMBER"
                  }
             }
	    
        stage('Docker Push') {
            // agent any
            steps {
		     docker.withRegistry( '', registryCredential ) {
        	     dockerImage.push()
             }
        }
      }
	    
        stage('Docker Tomcat server') {
              steps {
               		//bat 'docker stop mytomcat'
			//bat 'docker rm mytomcat'
			bat 'docker run -d --name mytomcat -p 9090:8080 shraddhal/tomcatserver'
            }
        }
	    
	 stage('archive') {
              steps {
               		archive 'target/*.war'
            }
        }    
    }
}
