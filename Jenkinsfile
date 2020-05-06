def config = [emailRecipients: 'kiran.vemula@sierra-cedar.com',
			anypointHost: 'anypoint.mulesoft.com',
			anypointEnv: 'dev',
			mavenImage: 'my_local_maven']

//All valid Declarative Pipelines must be enclosed within a pipeline block

pipeline
{
	//agent directive tells Jenkins where and how to execute the Pipeline
	
	agent {
		any {
			label 'my_label'
		}
	}
	
	
	//multi line comment
	//Execute the Pipeline, or stage, with the given container which will be dynamically provisioned on a node pre-configured to accept Docker-based Pipelines
	//docker also optionally accepts a registryUrl and registryCredentialsId parameters which will help to specify the Docker Registry to use and its credentials.
	
	/*agent { 
		docker 
			{ 
				image 'maven:3-alpine'
        		label 'my-defined-label'
        		args  '-v /tmp:/tmp'
        		registryUrl 'https://myregistry.com/'
        		registryCredentialsId 'myPredefinedCredentialsInJenkins'
			}
	}*/
	
	
	//Execute the Pipeline, or stage, with a container built from a Dockerfile contained in the source repository. 
	//In order to use this option, the Jenkinsfile must be loaded from either a Multibranch Pipeline or a Pipeline from SCM.
	
	/*
	agent {
	    // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
	    dockerfile {
	        filename 'Dockerfile.build'
	        dir 'build'
	        label 'my-defined-label'
	        additionalBuildArgs  '--build-arg version=1.0.2'
	        args '-v /tmp:/tmp'
	    }
	}
	*/
	
	
	
	
	//define tools
	
	tools {
        maven config.mavenImage  //my_local_maven is the same name used while configuring on Jenkins
    }
    
    
    
    
    
    
    //The options directive allows configuring Pipeline-specific options from within the Pipeline itself.
	//we can also set these at the stage level
	
	options {
        buildDiscarder(logRotator(numToKeepStr:'5'))
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
        //sendSplunkConsoleLog()
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
            
            
        //access credentials from Jenkins
        GITHUB_CREDS     = credentials('80571d5d-c411-4efe-bd63-66b008b72d4e')
        
            
    }
    
    
    
    
    
    //The parameters directive provides a list of parameters that a user should provide when triggering the Pipeline. 
    //The values for these user-specified parameters are made available to Pipeline steps via the params object
    
    /*
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    */
    
    
	stages {
	
		stage('Build Application') {
		
			//setting local environment variables - acccessible only in this stage
		
			environment { 
                DEBUG_FLAGS = '-g'
            }
		
			steps{
				
				echo "${GITHUB_CREDS}"
			
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
					echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
					
					
					
					sh 'mvn --version'
                    
                    sh 'mvn -U clean install'
				}	
			}
		}
		
		stage('Deploy Application to CloudHub') {
			steps{
				script {
					sh 'mvn package deploy -DmuleDeploy'
				}
			}
		}	
		
		
		//stage level agent section
		/*
		
		stage('Example Build') {
            agent { docker 'maven:3-alpine' } 
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
            }
        }
        stage('Example Test') {
            agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
        }
        
        */
        
        
        //An environment directive used in the top-level pipeline block will apply to all steps within the Pipeline.
		//An environment directive defined within a stage will only apply the given environment variables to steps within the stage.
		//The environment block has a helper method credentials() defined which can be used to access pre-defined Credentials by their identifier in the Jenkins environment.
        
        /*
        
        stage('Example Username/Password') {
            environment {
                SERVICE_CREDS = credentials('my-predefined-username-password')
            }
            steps {
                sh 'echo "Service user is $SERVICE_CREDS_USR"'
                sh 'echo "Service password is $SERVICE_CREDS_PSW"'
                sh 'curl -u $SERVICE_CREDS https://myservice.example.com'
            }
        }
        stage('Example Username/Password') {
            environment {
                SSH_CREDS = credentials('my-predefined-ssh-creds')
            }
            steps {
                sh 'echo "SSH private key is located at $SSH_CREDS"'
                sh 'echo "SSH user is $SSH_CREDS_USR"'
                sh 'echo "SSH passphrase is $SSH_CREDS_PSW"'
            }
        }
        
        */
        
        
        /*
        
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
        
        */
		
		
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
            mail to: config.emailRecipients,
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
             
             
            //email recipients:config.emailRecipients
             
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