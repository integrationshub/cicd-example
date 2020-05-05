pipeline
{
	agent any
	
	tools {
        maven 'my_local_maven' 
    }
    
	stages {
	
		stage('Build Application') {
			steps{
				script {
					sh 'echo "Hello World"'
	                sh '''
	                    echo "Multiline shell steps works too"
	                    ls -lah
	                '''
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
		
	}
}