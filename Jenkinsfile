pipeline {
    agent any
	tools{
		maven 'maven3.6'
		dockerTool 'docker'
	}
    environment {
        containerName = ""
        container_version = "1.0.0.${BUILD_ID}"
        dockerTag = "${containerName}:${container_version}"
	    
	registry = "shraddhal/tomcatserver-jenkinsanddocker-local"
    	registryCredential ='7d1e9b8f-6abf-4529-a30c-99f9173c2f2f'
    }
    stages{
        
        stage('Clone repository') {
			   steps {	       
				 checkout scm }
	}
        
       stage('Build') {
			   steps {	       
				  bat 'del filename  -- removes   target/roshambo.war'
                 		bat  'rmdir /s target/roshambo'
               			bat 'mvn clean package'
               } 
        
	}
        
        
        stage ('Build Container') {
            steps {
		    script{dockerImage  = docker.build registry + ":$BUILD_NUMBER"
			  }
                  }
             }
	    
        stage('Docker Push') {
            // agent any
            steps {
		    script{
			    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {

            app.push()
			    }
             }
        }
      }
	    
      /*  stage('Docker Tomcat server') {
              steps {
               		//bat 'docker stop mytomcat'
			//bat 'docker rm mytomcat'
			bat 'docker run -d --name mytomcat -p 9090:8080 registry + ":$BUILD_NUMBER"'
            }
        }
	    */
	 stage('archive') {
              steps {
               		archive 'target/*.war'
            }
        }    
    }
}
