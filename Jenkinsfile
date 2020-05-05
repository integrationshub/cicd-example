pipeline
{
	//agent directive tells Jenkins where and how to execute the Pipeline
	
	agent {
		any {
			label 'my_label'
		}
	}
	
	//multi line comment
	/*agent { 
		docker 
			{ 
				image 'maven:3.3.3' 
			}
	}*/
	
	//define tools
	
	tools {
        maven 'my_local_maven'  //my_local_maven is the same name used while configuring on Jenkins
    }
    
    //setting global environment variables - acccessible to all stages
    
    environment {
    
        DISABLE_AUTH = 'true'
        
        DB_ENGINE    = 'sqlite'
        
        // Using returnStdout
        CC = """${sh(
                returnStdout: true,
                script: 'echo "clang"'
            )}""" 
            
            
        // Using returnStatus
        EXIT_STATUS = """${sh(
                returnStatus: true,
                script: 'exit 1'
            )}"""
            
    }
    
	stages {
	
		stage('Build Application') {
		
			//setting local environment variables - acccessible only in this stage
		
			environment { 
                DEBUG_FLAGS = '-g'
            }
		
			steps{
			
				//echo "Database engine is ${DB_ENGINE}"
			
				//echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
			
				script {
				
					//sh 'echo "Hello World"'
					
	                /*sh '''
	                    echo "Multiline shell steps works too"
	                    ls -lah
	                '''
	                */
				
					//prints all environment variables
					sh 'printenv'
					
					//print only specific environment variables
					//sh 'echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"'
					
					
					
					sh 'mvn --version'
                    
                    //sh 'mvn -U clean install'
				}	
			}
		}
		
		/* stage('Deploy Application to CloudHub') {
			steps{
				script {
					sh 'mvn package deploy -DmuleDeploy'
				}
			}
		}	*/
		
		
		
	}
	
	
	
	post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
    
    
}